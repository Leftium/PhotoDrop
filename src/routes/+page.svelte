<script lang="ts">
    import '../app.scss';

    import { onMount } from 'svelte';

    import {
        PUBLIC_DROPBOX_APP_KEY,
        PUBLIC_DROPBOX_REQUEST_LINK,
        PUBLIC_DROPBOX_SHARED_LINK,
    } from '$env/static/public';

    let embedderDiv: HTMLElement;

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
        (window as any).Dropbox.embed(options, embedderDiv);
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
    <h1>
        <a href={PUBLIC_DROPBOX_REQUEST_LINK}>
            Share your photos from this event!
        </a>
    </h1>

    <hr />

    <h1>View photos from this event!</h1>

    <div class="embedder" bind:this={embedderDiv} />
</main>

<style>
    main {
        display: flex;
        flex-direction: column;

        height: 100%;
    }

    h1 {
        margin: auto;
    }

    .embedder {
        flex-grow: 1;
    }
</style>
