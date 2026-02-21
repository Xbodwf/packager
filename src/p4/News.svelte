<script>
  import Section from './Section.svelte';
  const color = '#b117f8';
</script>

<style>
  .news-content {
    padding: 4px 0;
  }
  
  .badge {
    display: inline-flex;
    align-items: center;
    background-color: #b117f8;
    color: white;
    padding: 4px 12px;
    border-radius: var(--md-sys-shape-corner-small, 8px);
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-label-large, 14px);
    font-weight: var(--md-sys-typescale-weight-medium, 500);
    letter-spacing: 0.5px;
    margin-bottom: 12px;
    box-shadow: var(--md-sys-elevation-level1, 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15));
  }
  
  .news-content :global(p) {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-large, 16px);
    line-height: 1.6;
    color: var(--md-sys-color-on-surface, #201A1A);
    margin: 8px 0;
  }
</style>

<Section accent={color}>
  <div class="news-content" lang="en">
    <p>
      <span class="badge">New!</span>
    </p>
    <p>
      By default, Electron apps will no longer be throttled when minimized or hidden.
      There is an option to restore the old behavior.
    </p>
  </div>
</Section>