<script lang="ts">
    export let name: string;
    export let url;
    export let environment;
    export let note;
    export let disabled = false;

    enum MonitorStatus {
        LOADING,
        AVAILABLE,
        UNAVAILABLE,
        DISABLED
    }

    interface ServiceStatus {
        commitHash: string;
        startupTime: string;
        version: string;
    }

    let monitorStatus = disabled ? MonitorStatus.DISABLED : MonitorStatus.LOADING;
    let isOnline = false;
    let isFunctional = false;
    let errorStatus: number;
    let serviceStatus: ServiceStatus;

    if (!disabled) {
        $: fetch(url).then(response => {
            isOnline = true;

            if (!response.ok) {
                errorStatus = response.status;
                throw new Error(`Services responded with invalid status: ${errorStatus}`);
            }

            return response.json();
        }).then(data => {
            serviceStatus = data;
            monitorStatus = MonitorStatus.AVAILABLE;
            isFunctional = true;
        }).catch(error => (monitorStatus = MonitorStatus.UNAVAILABLE));
    }
</script>

<main class="{disabled ? 'disabled' : ''}">
    <div class="caption">{name}</div>

    {#if note}
        <div class="note">{note}</div>
    {/if}

    <div class="meta-info">
        {#if url}
            <div>Url: {url}</div>
        {/if}
        {#if environment}
            <div>Env: {environment}</div>
        {/if}
    </div>

    <div class="content">
        {#if monitorStatus === MonitorStatus.LOADING}
            Loading...
        {:else if monitorStatus === MonitorStatus.DISABLED}
            Monitor disabled
        {:else}
            <div>Online: {isOnline}</div>
            {#if isOnline}
                <div>Functional: {isFunctional ? 'true' : `false (status: ${errorStatus})`}</div>
            {/if}

            <div class="service-details">
                {#if monitorStatus === MonitorStatus.AVAILABLE}
                    <div class="sub-caption">Service details</div>
                    <div class="service-details-list">
                        <div>Commit Hash: {serviceStatus.commitHash}</div>
                        <div>Startup time: {serviceStatus.startupTime}</div>
                        <div>Version: {serviceStatus.version}</div>
                    </div>
                {:else if monitorStatus === MonitorStatus.UNAVAILABLE}
                    Service Unavailable!
                {/if}
            </div>
        {/if}
    </div>
</main>

<style>
    main {
        margin: 20px;
        padding: 20px;
        min-width: 300px;
        background-color: #f4f4f4;
        border-radius: 10px;
        color: #333333;
        font-size: medium;
    }

    .caption {
        font-size: x-large;
    }

    .meta-info {
        margin-top: 15px;
    }

    .content {
        margin-top: 15px;
    }

    .note {
        margin: 15px 0 15px;
    }

    .disabled {
        color: #cccccc;
    }

    .sub-caption {
        font-weight: bold;
    }

    .service-details {
        margin-top: 10px;
    }

    .service-details-list {
        margin-top: 5px;
    }
</style>