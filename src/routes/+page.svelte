<script lang="ts">
    import '../app.scss';

    import QRCode from 'qrcode';

    import { onMount } from 'svelte';

    import { page } from '$app/stores';

    import {
        PUBLIC_DROPBOX_APP_KEY,
        PUBLIC_DROPBOX_REQUEST_LINK,
        PUBLIC_DROPBOX_SHARED_LINK,
    } from '$env/static/public';

    // Bindings:
    let embedderDiv: HTMLElement;

    let qrPageUrlDataUrl = QRCode.toDataURL($page.url.href);

    let qrGuestWifiDataUrl = QRCode.toDataURL(`WIFI:S:zoe_guest;`);

    let qrGuestWifi5gDataUrl = QRCode.toDataURL(`WIFI:S:zoe_guest_5G;`);

    onMount(() => {
        const options = {
            link: PUBLIC_DROPBOX_SHARED_LINK,
            file: {
                zoom: 'best',
            },
            folder: {
                view: 'grid',
                headerSize: 'small',
            },
        };
        // (window as any).Dropbox.embed(options, embedderDiv);
    });
</script>

<svelte:head>
    <script
        type="text/javascript"
        src="https://www.dropbox.com/static/api/2/dropins.js"
        id="dropboxjs"
        data-app-key={PUBLIC_DROPBOX_APP_KEY}
    ></script>
</svelte:head>

<main class="container">
    <p>
        <a href={PUBLIC_DROPBOX_REQUEST_LINK} role="button">
            Share your photos from this event!
        </a>
    </p>

    <p>
        <a href={PUBLIC_DROPBOX_SHARED_LINK} role="button">
            View photos from this event!
        </a>
    </p>

    <details>
        <summary role="button" class="secondary">
            Share this page with others:<br /><span class="url">
                {$page.url.href.replace(/^https?:\/\/(.*)\/$/i, '$1')}
            </span>
        </summary>
        {#await qrPageUrlDataUrl then dataUrl}
            <img class="qrcode" src={dataUrl} />
        {/await}
    </details>

    <div class="embedder" bind:this={embedderDiv} hidden />
    <hr />
    Connect to guest WiFi:

    <details>
        <summary role="button" class="secondary">zoe_guest_5G</summary>
        {#await qrGuestWifi5gDataUrl then dataUrl}
            <img class="qrcode" src={dataUrl} />
        {/await}
    </details>

    <details>
        <summary role="button" class="secondary">zoe_guest</summary>
        {#await qrGuestWifiDataUrl then dataUrl}
            <img class="qrcode" src={dataUrl} />
        {/await}
    </details>
</main>

<style>
    h1 {
        margin: auto;
    }

    details {
        max-width: 25em;
    }

    .url {
        font-family: monospace;
    }

    .embedder {
        flex-grow: 1;

        visibility: hidden;
    }
</style>
