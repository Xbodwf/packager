<script>
  import {_} from '../locales';
  import DropArea from './DropArea.svelte';

  const ACCEPT = [
    '.png',
    '.jpg',
    '.jpeg',
    '.bmp',
    '.svg',
    '.ico',
    '.gif'
  ];

  export let file;
  export let previewSizes;
  let dropping;
  let url;

  // This is a bit strange, there's probably a better way to do this
  // Seems to create and revoke an extra object URL for each file for some reason
  $: if (file) {
    if (url) {
      URL.revokeObjectURL(url);
    }
    url = URL.createObjectURL(file);
  } else if (url) {
    URL.revokeObjectURL(url);
    url = null;
  }

  const clear = (e) => {
    e.stopPropagation();
    file = null;
  };

  const handleClickBackground = () => {
    const input = document.createElement('input');
    input.type = 'file';
    input.accept = ACCEPT.join(',');
    input.addEventListener('change', (e) => {
      const files = e.target.files;
      if (files.length) {
        file = files[0];
      } else {
        file = null;
      }
    });
    document.body.appendChild(input);
    input.click();
    input.remove();
  };

  const handleDrop = ({detail: dataTransfer}) => {
    const droppedFile = dataTransfer.files[0];
    if (ACCEPT.some((ext) => droppedFile.name.endsWith(ext))) {
      file = droppedFile;
    }
  };
</script>

<style>
  .container {
    background: transparent;
    color: var(--md-sys-color-on-surface-variant, #534342);
    width: 100%;
    box-sizing: border-box;
    border: 2px dashed var(--md-sys-color-outline, #857372);
    transition: 
      border-color var(--md-sys-motion-duration-short3, 150ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      background-color var(--md-sys-motion-duration-short3, 150ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      color var(--md-sys-motion-duration-short3, 150ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    border-radius: var(--md-sys-shape-corner-medium, 12px);
    min-height: 100px;
    font: inherit;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    overflow: hidden;
    position: relative;
    cursor: pointer;
    padding: 16px;
  }
  
  .container:hover {
    border-color: var(--md-sys-color-primary, #BA1A1A);
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.04);
  }
  
  .dropping {
    border-color: var(--md-sys-color-primary, #BA1A1A);
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.08);
    color: var(--md-sys-color-primary, #BA1A1A);
    border-width: 3px;
  }
  
  .container:focus-visible {
    border-color: var(--md-sys-color-primary, #BA1A1A);
    border-width: 3px;
    outline: none;
  }
  
  .placeholder {
    font-size: var(--md-sys-typescale-body-large, 16px);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
  }
  
  .placeholder-icon {
    width: 48px;
    height: 48px;
    opacity: 0.5;
  }
  
  .selected {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    gap: 16px;
  }
  
  .preview-container {
    display: flex;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
  }
  
  .preview-container img {
    border-radius: var(--md-sys-shape-corner-small, 8px);
    box-shadow: var(--md-sys-elevation-level1, 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15));
  }
  
  .file-info {
    display: flex;
    flex-direction: column;
    gap: 8px;
    align-items: center;
  }
  
  .file-name {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    color: var(--md-sys-color-on-surface, #201A1A);
    word-break: break-all;
  }
  
  .clear-button {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-label-large, 14px);
    font-weight: var(--md-sys-typescale-weight-medium, 500);
    color: var(--md-sys-color-primary, #BA1A1A);
    background: transparent;
    border: 1px solid var(--md-sys-color-outline, #857372);
    padding: 8px 16px;
    border-radius: var(--md-sys-shape-corner-full, 9999px);
    cursor: pointer;
    transition: 
      background-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      border-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
  }
  
  .clear-button:hover {
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.08);
    border-color: var(--md-sys-color-primary, #BA1A1A);
  }
</style>

<DropArea bind:dropping={dropping} on:drop={handleDrop}>
  <button class="container" class:dropping on:click={handleClickBackground}>
    {#if file}
      <div class="selected">
        <div class="preview-container">
          {#each previewSizes as size}
            <!-- svelte-ignore a11y-missing-attribute -->
            <img src={url} width={size[0]} height={size[1]}>
          {/each}
        </div>
        <div class="file-info">
          <span class="file-name">{$_('fileInput.selected').replace('{file}', file.name)}</span>
          <button class="clear-button" on:click={clear}>{$_('fileInput.clear')}</button>
        </div>
      </div>
    {:else}
      <div class="placeholder">
        <svg class="placeholder-icon" viewBox="0 0 24 24" fill="currentColor">
          <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm0 16H5V5h14v14zm-5-7l-3 3.72L9 13l-3 4h12l-4-5z"/>
        </svg>
        <span>{$_('fileInput.select')}</span>
      </div>
    {/if}
  </button>
</DropArea>