<script lang="ts">
    export let name: string;
    export let url;
    export let environment;
    export let note;
    export let disabled = false;

    enum MonitorStatus {
        LOADING,
        OK,
        UNAVAILABLE,
        UNHEALTHY,
        DISABLED
    }

    interface ServiceStatus {
        commitHash: string;
        startupTime: string;
        version: string;
    }

    const getMonitorStatusClassName =
        (monitorStatus: MonitorStatus): string => {
            let result: string;

            switch (monitorStatus) {
                case MonitorStatus.OK:
                    result = 'ok';
                    break;
                case MonitorStatus.DISABLED:
                    result = 'disabled';
                    break;
                case MonitorStatus.UNHEALTHY:
                    result = 'unhealthy';
                    break;
                case MonitorStatus.UNAVAILABLE:
                    result = 'unavailable';
                    break;
                case MonitorStatus.LOADING:
                    result = 'loading';
                    break;
            }

            return result;
        }

    let monitorStatus = disabled ? MonitorStatus.DISABLED : MonitorStatus.LOADING;
    let isOnline = false;
    let isFunctional = false;
    let errorStatus: number;
    let serviceStatus: ServiceStatus;

    $: classNames = getMonitorStatusClassName(monitorStatus);

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
            monitorStatus = MonitorStatus.OK;
            isFunctional = true;
        }).catch(error => (monitorStatus = MonitorStatus.UNAVAILABLE));
    }
</script>

<main class={classNames}>
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
                {#if monitorStatus === MonitorStatus.OK}
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

    main.disabled {
        color: #cccccc;
    }

    main.ok {
        background-color: greenyellow;
    }

    main.unhealthy {
        background-color: orange;
    }

    main.unavailable {
        background-color: orangered;
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