<script>
  import ResetButton from './ResetButton.svelte';

  export let caption = false;
  export let center = false;
  export let modal = false;
  export let accent = '';
  export let reset;
</script>

<style>
  .card {
    max-width: 625px;
    margin: auto;
    border-radius: var(--md-sys-shape-corner-medium, 12px);
    background-color: var(--md-sys-color-surface, #FFFFFF);
    color: var(--md-sys-color-on-surface, #1C1B1F);
    padding: 16px 20px;
    margin-top: 8px;
    margin-bottom: 8px;
    box-shadow: var(--md-sys-elevation-level1, 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15));
    border: 1px solid var(--md-sys-color-outline-variant, #CAC4D0);
    transition: 
      box-shadow var(--md-sys-motion-duration-short4, 200ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      background-color var(--md-sys-motion-duration-short4, 200ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
  }
  
  .card:hover {
    box-shadow: var(--md-sys-elevation-level2, 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 2px 6px 2px rgba(0, 0, 0, 0.15));
  }
  
  .caption {
    font-style: italic;
    color: var(--md-sys-color-on-surface-variant, #49454F);
  }
  
  .modal {
    width: 100%;
    max-width: 480px;
    margin: 0 16px;
    background-color: var(--md-sys-color-surface-container-high, #EDEDED);
  }
  
  .center {
    min-height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  
  .reset {
    float: right;
  }
  
  .accent-border {
    border-top: 3px solid var(--md-brand-accent, #FF4C4C);
    border-top-left-radius: var(--md-sys-shape-corner-medium, 12px);
    border-top-right-radius: var(--md-sys-shape-corner-medium, 12px);
  }
</style>

<div
  class="card"
  class:caption
  class:modal
  class:center={caption || center}
  class:accent-border={accent}
  style:--md-brand-accent={accent || 'var(--md-sys-color-primary, #C00100)'}
>
  {#if reset}
    <div class="reset">
      <ResetButton on:click={reset} />
    </div>
  {/if}
  <slot></slot>
</div>
