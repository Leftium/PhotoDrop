<script lang="ts">
  import "../app.scss";

  import QRCode from "qrcode";

  import { page } from "$app/stores";

  import {
    PUBLIC_DROPBOX_REQUEST_LINK,
    PUBLIC_SLIDESHOW_LANDSCAPE_LINK,
    PUBLIC_SLIDESHOW_PORTRAIT_LINK,
  } from "$env/static/public";

  // Bindings:
  let embedderDiv: HTMLElement;

  let qrPageUrlDataUrl = QRCode.toDataURL($page.url.href);

  let qrGuestWifiDataUrl = QRCode.toDataURL(`WIFI:S:party_guest;`);

  let qrGuestWifi5gDataUrl = QRCode.toDataURL(`WIFI:S:party_guest_5G;`);
</script>

<main class="container">
  <h4>Share your photos from this event!</h4>
  <p>
    <a href={PUBLIC_DROPBOX_REQUEST_LINK} role="button">
      Open Dropbox request
    </a>
  </p>

  <h4>View photos from this event!</h4>

  <p>
    <a href={PUBLIC_SLIDESHOW_PORTRAIT_LINK} role="button">Portrait View</a>
  </p>
  <p>
    <a href={PUBLIC_SLIDESHOW_LANDSCAPE_LINK} role="button">Landscape View</a>
  </p>

  <hr />

  <h4>Share this page with others:</h4>
  <details>
    <summary role="button" class="secondary">
      <span class="url">
        {$page.url.href.replace(/^https?:\/\/(.*)\/$/i, "$1")}
      </span>
    </summary>
    {#await qrPageUrlDataUrl then dataUrl}
      <img class="qrcode" src={dataUrl} />
    {/await}
  </details>

  <div class="embedder" bind:this={embedderDiv} hidden />
  <hr />

  <h4>Guest Wi-Fi Networks:</h4>

  <details>
    <summary role="button" class="secondary outline">party_guest_5G</summary>
    {#await qrGuestWifi5gDataUrl then dataUrl}
      <img class="qrcode" src={dataUrl} />
    {/await}
  </details>

  <details>
    <summary role="button" class="secondary outline">party_guest</summary>
    {#await qrGuestWifiDataUrl then dataUrl}
      <img class="qrcode" src={dataUrl} />
    {/await}
  </details>
</main>

<style>
  main {
    max-width: 25em;
    margin: 4px auto;
  }

  [role="button"] {
    text-align: left;
    width: 100%;
  }

  .url {
    font-family: monospace;
  }

  .embedder {
    flex-grow: 1;

    visibility: hidden;
  }
</style>
