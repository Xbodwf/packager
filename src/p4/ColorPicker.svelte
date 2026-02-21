<script>
  export let value;
  export let label = '';
</script>

<style>
  .color-input-wrapper {
    display: inline-flex;
    align-items: center;
    gap: 12px;
  }
  
  input[type="color"] {
    -webkit-appearance: none;
    appearance: none;
    width: 48px;
    height: 48px;
    padding: 0;
    border: 2px solid var(--md-sys-color-outline-variant, #D8C2C0);
    border-radius: var(--md-sys-shape-corner-small, 8px);
    cursor: pointer;
    background: transparent;
    transition: 
      border-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      box-shadow var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    overflow: hidden;
  }
  
  input[type="color"]::-webkit-color-swatch-wrapper {
    padding: 4px;
  }
  
  input[type="color"]::-webkit-color-swatch {
    border: none;
    border-radius: var(--md-sys-shape-corner-extra-small, 4px);
  }
  
  input[type="color"]::-moz-color-swatch {
    border: none;
    border-radius: var(--md-sys-shape-corner-extra-small, 4px);
  }
  
  input[type="color"]:hover {
    border-color: var(--md-sys-color-outline, #857372);
  }
  
  input[type="color"]:focus {
    border-color: var(--md-sys-color-primary, #BA1A1A);
    border-width: 2px;
    outline: none;
  }
  
  .label {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    color: var(--md-sys-color-on-surface-variant, #534342);
  }
</style>

<div class="color-input-wrapper">
  <input type="color" bind:value />
  {#if label}
    <span class="label">{label}</span>
  {/if}
</div>