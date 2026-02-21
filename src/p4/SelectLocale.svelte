<script>
  import {locale, localeNames} from '../locales/index';
</script>

<style>
  .select-wrapper {
    position: relative;
    display: inline-flex;
    align-items: center;
  }
  
  select {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    padding: 8px 40px 8px 16px;
    background-color: transparent;
    color: var(--md-sys-color-on-surface, #201A1A);
    border: 1px solid var(--md-sys-color-outline, #857372);
    border-radius: var(--md-sys-shape-corner-extra-small, 4px);
    cursor: pointer;
    appearance: none;
    min-width: 140px;
    transition: 
      border-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      background-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    outline: none;
  }
  
  select:hover {
    border-color: var(--md-sys-color-on-surface, #201A1A);
  }
  
  select:focus {
    border-color: var(--md-sys-color-primary, #BA1A1A);
    border-width: 2px;
    padding-left: 15px;
  }
  
  .dropdown-icon {
    position: absolute;
    right: 12px;
    pointer-events: none;
    color: var(--md-sys-color-on-surface-variant, #534342);
    width: 20px;
    height: 20px;
  }
</style>

<div class="select-wrapper">
  <select bind:value={$locale}>
    {#each Object.entries(localeNames) as [locale, name]}
      <option value={locale}>{name || locale}</option>
    {/each}
  </select>
  <svg class="dropdown-icon" viewBox="0 0 24 24" fill="currentColor">
    <path d="M7 10l5 5 5-5z"/>
  </svg>
</div>