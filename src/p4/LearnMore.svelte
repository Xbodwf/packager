<script>
  import {_} from '../locales/';

  export let slug;
  export let href;

  const link = slug ? `https://docs.turbowarp.org/${slug}` : href;
</script>

<style>
  .learn-more-link {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 20px;
    height: 20px;
    text-decoration: none;
    color: var(--md-sys-color-on-surface-variant, #534342);
    background-color: var(--md-sys-color-surface-container-highest, #F3E5E4);
    border-radius: var(--md-sys-shape-corner-full, 9999px);
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-label-small, 11px);
    font-weight: var(--md-sys-typescale-weight-medium, 500);
    transition: 
      background-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      transform var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    margin-left: 4px;
    vertical-align: middle;
  }
  
  .learn-more-link:hover {
    background-color: var(--md-sys-color-primary, #BA1A1A);
    color: var(--md-sys-color-on-primary, #FFFFFF);
    text-decoration: none;
    transform: scale(1.1);
  }
  
  .learn-more-link:focus-visible {
    outline: 2px solid var(--md-sys-color-primary, #BA1A1A);
    outline-offset: 2px;
  }
</style>

<a class="learn-more-link" href={link} title={$_('options.learnMore')} target="_blank" rel="noopener noreferrer">?</a>