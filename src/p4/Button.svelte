<script>
  import {ACCENT_COLOR} from '../packager/brand';
  export let secondary;
  export let dangerous;
  export let text;
  export let outlined;  // New: outlined variant
  export let textual;   // New: text-only variant

  // M3 button variants:
  // - Filled (default): primary action
  // - Outlined: secondary action
  // - Text (textual): tertiary action
  
  const getColors = () => {
    if (secondary) {
      return {
        bg: 'var(--md-brand-secondary, #0FBD8C)',
        on: 'var(--md-brand-secondary-on, #FFFFFF)'
      };
    }
    if (dangerous) {
      return {
        bg: 'var(--md-brand-warning, #FF8C1A)',
        on: 'var(--md-brand-warning-on, #000000)'
      };
    }
    return {
      bg: `var(--md-brand-accent, ${ACCENT_COLOR})`,
      on: 'var(--md-brand-accent-on, #FFFFFF)'
    };
  };
</script>

<style>
  button {
    position: relative;
    font-family: var(--md-ref-typeface-plain, 'Roboto', sans-serif);
    font-size: var(--md-sys-typescale-label-large, 14px);
    font-weight: var(--md-sys-typescale-weight-medium, 500);
    letter-spacing: 0.1px;
    color: white;
    border: none;
    padding: 0 24px;
    height: var(--md-comp-button-container-height, 40px);
    min-width: 48px;
    margin: 4px;
    border-radius: var(--md-comp-button-filled-container-shape, 9999px);
    overflow: hidden;
    cursor: pointer;
    transition: 
      background-color var(--md-sys-motion-duration-short3, 150ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      box-shadow var(--md-sys-motion-duration-short3, 150ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    outline: none;
  }
  
  /* Filled button (default) */
  button:not(.outlined):not(.textual) {
    box-shadow: var(--md-sys-elevation-level1, 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15));
  }
  
  button:not(.outlined):not(.textual):hover {
    box-shadow: var(--md-sys-elevation-level2, 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 2px 6px 2px rgba(0, 0, 0, 0.15));
  }
  
  /* Outlined button */
  button.outlined {
    background-color: transparent;
    border: 1px solid var(--md-sys-color-outline, #857372);
    color: var(--md-sys-color-primary, #BA1A1A);
    box-shadow: none;
  }
  
  button.outlined:hover {
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.08);
  }
  
  /* Text button */
  button.textual {
    background-color: transparent;
    color: var(--md-sys-color-primary, #BA1A1A);
    box-shadow: none;
    padding: 0 12px;
  }
  
  button.textual:hover {
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.08);
  }
  
  /* State layer */
  .state-layer {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: currentColor;
    opacity: 0;
    transition: opacity var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    pointer-events: none;
  }
  
  button:hover .state-layer {
    opacity: var(--md-sys-state-hover-opacity, 0.08);
  }
  
  button:focus-visible .state-layer {
    opacity: var(--md-sys-state-focus-opacity, 0.12);
  }
  
  button:active .state-layer {
    opacity: var(--md-sys-state-pressed-opacity, 0.12);
  }
  
  .text {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    z-index: 1;
    white-space: nowrap;
  }
  
  /* Disabled state */
  button:disabled {
    cursor: default;
    pointer-events: none;
    opacity: var(--md-sys-state-disabled-opacity, 0.38);
  }
  
  button:disabled .state-layer {
    display: none;
  }
  
  button.outlined:disabled {
    border-color: rgba(var(--md-sys-color-on-surface-rgb, 32, 26, 26), 0.12);
    color: rgba(var(--md-sys-color-on-surface-rgb, 32, 26, 26), 0.38);
  }
</style>

<button 
  on:click 
  class:outlined 
  class:textual
  style:background-color={outlined || textual ? undefined : getColors().bg}
  style:color={outlined || textual ? undefined : getColors().on}
>
  <div class="state-layer"></div>
  <div class="text">{text}</div>
</button>