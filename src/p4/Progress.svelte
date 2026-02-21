<script>
  export let progress = 0;
  export let text = '';
</script>

<style>
  .progress-container {
    display: flex;
    align-items: center;
    flex-direction: column;
    gap: 12px;
    width: 100%;
    max-width: 280px;
  }
  
  .progress-track {
    width: 100%;
    height: var(--md-comp-progress-indicator-linear-track-height, 4px);
    background-color: var(--md-sys-color-surface-container-highest, #F3E5E4);
    border-radius: var(--md-sys-shape-corner-full, 9999px);
    overflow: hidden;
    position: relative;
  }
  
  .progress-bar {
    height: 100%;
    width: 0;
    background-color: var(--md-sys-color-primary, #BA1A1A);
    border-radius: var(--md-sys-shape-corner-full, 9999px);
    transition: width var(--md-sys-motion-duration-medium2, 300ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    position: relative;
    overflow: hidden;
  }
  
  /* Indeterminate animation for when progress is 0 */
  @keyframes indeterminate {
    0% {
      transform: translateX(-100%);
    }
    100% {
      transform: translateX(200%);
    }
  }
  
  .progress-bar.indeterminate {
    width: 50%;
    animation: indeterminate 1.5s ease-in-out infinite;
  }
  
  .progress-text {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    color: var(--md-sys-color-on-surface-variant, #534342);
    text-align: center;
    font-style: italic;
  }
</style>

<div class="progress-container">
  <div class="progress-track">
    <div 
      class="progress-bar"
      class:indeterminate={progress === 0}
      style:width={progress > 0 ? `${progress * 100}%` : undefined}
    ></div>
  </div>
  {#if text}
    <div class="progress-text">
      {text}
    </div>
  {/if}
</div>