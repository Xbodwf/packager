<script>
  import {_} from '../locales/';
  import {fade} from 'svelte/transition';
  import ComplexMessage from './ComplexMessage.svelte';
  import Section from './Section.svelte';
  import SelectProject from './SelectProject.svelte';
  import SelectLocale from './SelectLocale.svelte';
  import SelectTheme from './SelectTheme.svelte';
  import Progress from './Progress.svelte';
  import Modals from './Modals.svelte';
  import News from './News.svelte';
  import {progress, theme, error} from './stores';
  import {isSupported, isSafari, isStandalone, version} from './environment';
  import {
    APP_NAME,
    FEEDBACK_PRIMARY,
    FEEDBACK_SECONDARY,
    ACCENT_COLOR,
    SOURCE_CODE,
    WEBSITE,
    DONATE,
    PRIVACY_POLICY
  } from '../packager/brand';

  let projectData;

  const darkMedia = window.matchMedia('(prefers-color-scheme: dark)');
  let systemTheme = darkMedia.matches ? 'dark' : 'light';
  if (darkMedia.addEventListener) {
    darkMedia.addEventListener('change', () => {
      systemTheme = darkMedia.matches ? 'dark' : 'light';
    });
  }
  $: document.documentElement.setAttribute('theme', $theme === 'system' ? systemTheme : $theme);

  let modalVisible = false;

  const defaultTitle = document.title;
  let title = '';
  $: document.title = projectData && title ? `${title} - ${APP_NAME}` : defaultTitle;

  const getPackagerOptionsComponent = () => import(
    /* webpackChunkName: "packager-options-ui" */
    './PackagerOptions.svelte'
  ).catch((err) => {
    $error = err;
  });

  // We know for sure we will need this component very soon, so start loading it immediately.
  getPackagerOptionsComponent();
</script>

<style>
  /* ========================================
   * Material Design 3 Theme System
   * Based on Google's Material Design 3 specifications
   * https://m3.material.io/
   * ======================================== */
  
  :global(:root) {
    /* Typography - M3 Type Scale with Lexend font */
    --md-ref-typeface-brand: 'Lexend', 'Noto Sans SC', 'Noto Sans TC', system-ui, -apple-system, sans-serif;
    --md-ref-typeface-plain: 'Lexend', 'Noto Sans SC', 'Noto Sans TC', system-ui, -apple-system, sans-serif;
    
    --md-sys-typescale-display-large: 57px;
    --md-sys-typescale-display-medium: 45px;
    --md-sys-typescale-display-small: 36px;
    --md-sys-typescale-headline-large: 32px;
    --md-sys-typescale-headline-medium: 28px;
    --md-sys-typescale-headline-small: 24px;
    --md-sys-typescale-title-large: 22px;
    --md-sys-typescale-title-medium: 16px;
    --md-sys-typescale-title-small: 14px;
    --md-sys-typescale-body-large: 16px;
    --md-sys-typescale-body-medium: 14px;
    --md-sys-typescale-body-small: 12px;
    --md-sys-typescale-label-large: 14px;
    --md-sys-typescale-label-medium: 12px;
    --md-sys-typescale-label-small: 11px;
    
    /* Font weights */
    --md-sys-typescale-weight-regular: 400;
    --md-sys-typescale-weight-medium: 500;
    --md-sys-typescale-weight-bold: 700;
    
    /* Line heights */
    --md-sys-typescale-line-height-display: 1.12;
    --md-sys-typescale-line-height-headline: 1.25;
    --md-sys-typescale-line-height-title: 1.43;
    --md-sys-typescale-line-height-body: 1.5;
    --md-sys-typescale-line-height-label: 1.43;

    /* Shape - M3 Corner Radius */
    --md-sys-shape-corner-none: 0;
    --md-sys-shape-corner-extra-small: 4px;
    --md-sys-shape-corner-small: 8px;
    --md-sys-shape-corner-medium: 12px;
    --md-sys-shape-corner-large: 16px;
    --md-sys-shape-corner-extra-large: 28px;
    --md-sys-shape-corner-full: 9999px;

    /* Elevation - M3 Shadows */
    --md-sys-elevation-level1: 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
    --md-sys-elevation-level2: 0px 1px 2px rgba(0, 0, 0, 0.3), 0px 2px 6px 2px rgba(0, 0, 0, 0.15);
    --md-sys-elevation-level3: 0px 4px 8px 3px rgba(0, 0, 0, 0.15), 0px 1px 3px rgba(0, 0, 0, 0.3);

    /* Motion - M3 Duration */
    --md-sys-motion-duration-short2: 100ms;
    --md-sys-motion-duration-short3: 150ms;
    --md-sys-motion-duration-short4: 200ms;
    --md-sys-motion-duration-medium1: 250ms;
    --md-sys-motion-duration-medium2: 300ms;
    --md-sys-motion-easing-standard: cubic-bezier(0.2, 0, 0, 1);
    --md-sys-motion-easing-emphasized-decelerate: cubic-bezier(0.05, 0.7, 0.1, 1);

    /* State Layers - Opacity */
    --md-sys-state-hover-opacity: 0.08;
    --md-sys-state-focus-opacity: 0.12;
    --md-sys-state-pressed-opacity: 0.12;
    --md-sys-state-disabled-opacity: 0.38;

    /* Component Dimensions */
    --md-comp-button-container-height: 40px;
    --md-comp-card-container-shape: 12px;
    --md-comp-dialog-container-shape: 28px;
    --md-comp-progress-indicator-linear-track-height: 4px;

    /* ========================================
     * Light Theme Colors - Neutral palette
     * ======================================== */
    --md-sys-color-primary: #C00100;
    --md-sys-color-on-primary: #FFFFFF;
    --md-sys-color-primary-container: #FFE8E6;
    --md-sys-color-on-primary-container: #410002;
    
    --md-sys-color-secondary: #5C5C5C;
    --md-sys-color-on-secondary: #FFFFFF;
    
    --md-sys-color-error: #BA1A1A;
    --md-sys-color-on-error: #FFFFFF;
    --md-sys-color-error-container: #FFDAD6;
    --md-sys-color-on-error-container: #410002;
    
    --md-sys-color-background: #FAFAFA;
    --md-sys-color-on-background: #1C1B1F;
    --md-sys-color-surface: #FAFAFA;
    --md-sys-color-on-surface: #1C1B1F;
    --md-sys-color-surface-variant: #E7E0EC;
    --md-sys-color-on-surface-variant: #49454F;
    
    --md-sys-color-outline: #79747E;
    --md-sys-color-outline-variant: #CAC4D0;
    --md-sys-color-scrim: rgba(0, 0, 0, 0.32);
    
    --md-sys-color-surface-container-lowest: #FFFFFF;
    --md-sys-color-surface-container-low: #F7F7F7;
    --md-sys-color-surface-container: #F3F3F3;
    --md-sys-color-surface-container-high: #EDEDED;
    --md-sys-color-surface-container-highest: #E6E6E6;

    /* Custom brand accent colors */
    --md-brand-accent: #FF4C4C;
    --md-brand-accent-on: #FFFFFF;
    --md-brand-secondary: #0FBD8C;
    --md-brand-secondary-on: #FFFFFF;
    --md-brand-warning: #FF8C1A;
    --md-brand-warning-on: #000000;
    
    /* Base styles */
    font-family: var(--md-ref-typeface-plain);
    background: var(--md-sys-color-background);
    color: var(--md-sys-color-on-background);
    scroll-behavior: smooth;
  }
  
  /* ========================================
   * Dark Theme Colors - Neutral palette
   * ======================================== */
  :global([theme="dark"]) {
    --md-sys-color-primary: #FF8A80;
    --md-sys-color-on-primary: #690000;
    --md-sys-color-primary-container: #930000;
    --md-sys-color-on-primary-container: #FFDAD6;
    
    --md-sys-color-secondary: #CBCBCB;
    --md-sys-color-on-secondary: #333333;
    
    --md-sys-color-error: #FFB4AB;
    --md-sys-color-on-error: #690005;
    --md-sys-color-error-container: #93000A;
    --md-sys-color-on-error-container: #FFDAD6;
    
    --md-sys-color-background: #1C1B1F;
    --md-sys-color-on-background: #E6E1E5;
    --md-sys-color-surface: #1C1B1F;
    --md-sys-color-on-surface: #E6E1E5;
    --md-sys-color-surface-variant: #49454F;
    --md-sys-color-on-surface-variant: #CAC4D0;
    
    --md-sys-color-outline: #938F99;
    --md-sys-color-outline-variant: #49454F;
    --md-sys-color-scrim: rgba(0, 0, 0, 0.48);
    
    --md-sys-color-surface-container-lowest: #0F0F0F;
    --md-sys-color-surface-container-low: #1D1D1D;
    --md-sys-color-surface-container: #212121;
    --md-sys-color-surface-container-high: #2B2B2B;
    --md-sys-color-surface-container-highest: #363636;

    /* Custom brand accent colors - dark mode */
    --md-brand-accent: #FF6B6B;
    --md-brand-accent-on: #000000;
    --md-brand-secondary: #2DD4A3;
    --md-brand-secondary-on: #000000;
    --md-brand-warning: #FFB347;
    --md-brand-warning-on: #000000;
    
    background: var(--md-sys-color-background);
    color: var(--md-sys-color-on-background);
    color-scheme: dark;
  }
  
  :global([theme="dark"]) {
    background: var(--md-sys-color-background);
    color: var(--md-sys-color-on-background);
    color-scheme: dark;
  }
  
  /* Typography - M3 Headline styles */
  :global(h1) {
    font-family: var(--md-ref-typeface-brand);
    font-size: var(--md-sys-typescale-headline-large);
    font-weight: var(--md-sys-typescale-weight-regular);
    line-height: var(--md-sys-typescale-line-height-headline);
    color: var(--md-sys-color-on-surface);
    margin: 16px 0 8px 0;
  }
  
  :global(h2) {
    font-family: var(--md-ref-typeface-brand);
    font-size: var(--md-sys-typescale-title-large);
    font-weight: var(--md-sys-typescale-weight-regular);
    line-height: var(--md-sys-typescale-line-height-title);
    color: var(--md-sys-color-on-surface);
    margin: 12px 0 4px 0;
  }
  
  :global(h3) {
    font-family: var(--md-ref-typeface-brand);
    font-size: var(--md-sys-typescale-title-medium);
    font-weight: var(--md-sys-typescale-weight-medium);
    line-height: var(--md-sys-typescale-line-height-title);
    color: var(--md-sys-color-on-surface);
    margin: 8px 0 4px 0;
  }
  
  :global(p) {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    line-height: 1.4;
    color: var(--md-sys-color-on-surface);
    margin: 6px 0;
  }
  
  /* Links - M3 style */
  :global(a) {
    color: var(--md-sys-color-primary);
    text-decoration: none;
    font-weight: var(--md-sys-typescale-weight-medium);
    transition: color var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard);
  }
  
  :global(a:hover) {
    text-decoration: underline;
  }
  
  :global(a:focus-visible) {
    outline: 2px solid var(--md-sys-color-primary);
    outline-offset: 2px;
    border-radius: 2px;
  }
  
  :global(a:active) {
    color: var(--md-sys-color-on-primary-container);
  }
  
  /* Form elements - M3 Outlined Text Field style */
  :global(input[type="text"]),
  :global(input[type="number"]),
  :global(input[type="url"]),
  :global(input[type="email"]),
  :global(textarea) {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    padding: 10px 12px;
    background-color: transparent;
    color: var(--md-sys-color-on-surface);
    border: 1px solid var(--md-sys-color-outline);
    border-radius: var(--md-sys-shape-corner-extra-small, 4px);
    transition: 
      border-color var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard),
      background-color var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard);
    outline: none;
    min-height: 20px;
  }
  
  :global(input[type="text"]:hover),
  :global(input[type="number"]:hover),
  :global(input[type="url"]:hover),
  :global(input[type="email"]:hover),
  :global(textarea:hover) {
    border-color: var(--md-sys-color-on-surface);
  }
  
  :global(input[type="text"]:focus),
  :global(input[type="number"]:focus),
  :global(input[type="url"]:focus),
  :global(input[type="email"]:focus),
  :global(textarea:focus) {
    border-color: var(--md-sys-color-primary);
    border-width: 2px;
    padding: 9px 11px;
  }
  
  :global([theme="dark"] input[type="text"]),
  :global([theme="dark"] input[type="number"]),
  :global([theme="dark"] input[type="url"]),
  :global([theme="dark"] input[type="email"]),
  :global([theme="dark"] textarea) {
    background-color: transparent;
    color: var(--md-sys-color-on-surface);
    border-color: var(--md-sys-color-outline);
  }
  
  :global([theme="dark"] input[type="text"]:hover),
  :global([theme="dark"] input[type="number"]:hover),
  :global([theme="dark"] input[type="url"]:hover),
  :global([theme="dark"] input[type="email"]:hover),
  :global([theme="dark"] textarea:hover) {
    border-color: var(--md-sys-color-on-surface);
  }
  
  /* Select - M3 Outlined style */
  :global(.is-not-safari select) {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-medium, 14px);
    padding: 8px 36px 8px 12px;
    background-color: transparent;
    color: var(--md-sys-color-on-surface);
    border: 1px solid var(--md-sys-color-outline);
    border-radius: var(--md-sys-shape-corner-extra-small, 4px);
    cursor: pointer;
    appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24'%3E%3Cpath fill='%2379747E' d='M7 10l5 5 5-5z'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 8px center;
    background-size: 20px;
    transition: 
      border-color var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard),
      background-color var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard);
    outline: none;
    min-height: 20px;
  }
  
  :global(.is-not-safari select:hover) {
    border-color: var(--md-sys-color-on-surface);
  }
  
  :global(.is-not-safari select:focus) {
    border-color: var(--md-sys-color-primary);
    border-width: 2px;
  }
  
  :global([theme="dark"] .is-not-safari select) {
    background-color: transparent;
    color: var(--md-sys-color-on-surface);
    border-color: var(--md-sys-color-outline);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='24' height='24' viewBox='0 0 24 24'%3E%3Cpath fill='%23938F99' d='M7 10l5 5 5-5z'/%3E%3C/svg%3E");
  }
  
  /* Checkbox and Radio - M3 style */
  :global(input[type="checkbox"]),
  :global(input[type="radio"]) {
    width: 18px;
    height: 18px;
    margin: 0 6px 0 0;
    accent-color: var(--md-sys-color-primary);
    cursor: pointer;
  }
  
  /* Details/Summary */
  :global(summary) {
    cursor: pointer;
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-large);
    color: var(--md-sys-color-primary);
    padding: 8px 0;
    list-style: none;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  
  :global(summary::before) {
    content: '';
    display: inline-block;
    width: 0;
    height: 0;
    border-left: 5px solid transparent;
    border-right: 5px solid transparent;
    border-top: 6px solid currentColor;
    transition: transform var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard);
  }
  
  :global(details[open] summary::before) {
    transform: rotate(180deg);
  }
  
  :global(summary:focus-visible) {
    outline: 2px solid var(--md-sys-color-primary);
    outline-offset: 2px;
    border-radius: 4px;
  }
  
  :global(summary::-webkit-details-marker) {
    display: none;
  }
  
  /* Code/pre elements */
  :global(code) {
    font-family: 'Roboto Mono', monospace;
    font-size: var(--md-sys-typescale-body-small);
    background-color: var(--md-sys-color-surface-container-highest);
    color: var(--md-sys-color-on-surface);
    padding: 2px 6px;
    border-radius: var(--md-sys-shape-corner-extra-small);
  }
  
  /* Main layout */
  main {
    padding: 16px 16px 24px 16px;
    min-height: 100vh;
    background-color: var(--md-sys-color-background);
  }
  
  /* Footer */
  footer {
    text-align: center;
    padding: 24px 16px;
    margin-top: 16px;
  }
  
  footer > div {
    margin-top: 12px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    gap: 4px;
  }
  
  footer a {
    padding: 8px 12px;
    border-radius: var(--md-sys-shape-corner-small);
    transition: background-color var(--md-sys-motion-duration-short2) var(--md-sys-motion-easing-standard);
  }
  
  footer a:hover {
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.08);
    text-decoration: none;
  }
  
  /* Utility classes */
  .disclaimer {
    font-style: italic;
    color: var(--md-sys-color-on-surface-variant);
    font-size: var(--md-sys-typescale-body-small);
  }
  
  .version {
    font-size: var(--md-sys-typescale-body-small);
    color: var(--md-sys-color-on-surface-variant);
    opacity: 0.8;
  }
  
  .version a {
    color: inherit;
    font-weight: normal;
  }
</style>

<Modals bind:modalVisible={modalVisible} />

<main aria-hidden={modalVisible} class:is-not-safari={!isSafari}>
  <Section accent={ACCENT_COLOR}>
    <div>
      <h1>{APP_NAME}</h1>
      {#if version}
        <p class="version">
          {version}
          {#if isStandalone}
            - <a href={WEBSITE}>{WEBSITE}</a>
          {/if}
        </p>
      {/if}
      <p>{$_('p4.description1')}</p>
      <p>
        <ComplexMessage
          message={$_('p4.description2')}
          values={{
            embedding: {
              text: $_('p4.description2-embedding'),
              href: 'https://docs.turbowarp.org/embedding'
            }
          }}
        />
      </p>
      <p>
        <ComplexMessage
          message={$_('p4.description3')}
          values={{
            // These placeholders are named this way for legacy reasons.
            onScratch: {
              text: $_('p4.description3-on').replace('{brand}', FEEDBACK_PRIMARY.name),
              href: FEEDBACK_PRIMARY.link
            },
            onGitHub: {
              text: $_('p4.description3-on').replace('{brand}', FEEDBACK_SECONDARY.name),
              href: FEEDBACK_SECONDARY.link
            }
          }}
        />
      </p>
      <p class="disclaimer">
        {$_('p4.disclaimer')}
      </p>
    </div>
  </Section>

  {#if !isStandalone}
    <News />
  {/if}

  {#if isSupported}
    <SelectProject bind:projectData />
  {:else}
    <Section accent="#4C97FF">
      <h2>{$_('p4.browserNotSupported')}</h2>
      <p>{$_('p4.browserNotSupportedDescription')}</p>
    </Section>
  {/if}

  {#if projectData}
    {#await getPackagerOptionsComponent()}
      <Section center>
        <Progress text={$_('p4.importingInterface')} />
      </Section>
    {:then { default: PackagerOptions }}
      <div in:fade>
        <PackagerOptions
          projectData={projectData}
          bind:title={title}
        />
      </div>
    {:catch}
      <Section center>
        <p>
          {$_('p4.unknownImportError')}
        </p>
      </Section>
    {/await}
  {/if}

  {#if $progress.visible}
    <Section center>
      <Progress progress={$progress.progress} text={$progress.text} />
    </Section>
  {/if}

  <footer>
    <div>
      {#if PRIVACY_POLICY && !isStandalone}
        <a href={PRIVACY_POLICY}>{$_('p4.privacy')}</a>
        <span>·</span>
      {/if}
      <a href={FEEDBACK_PRIMARY.link}>{$_('p4.feedback')}</a>
      {#if SOURCE_CODE}
        <span>·</span>
        <a href={SOURCE_CODE}>{$_('p4.sourceCode')}</a>
      {/if}
      {#if DONATE}
        <!-- Donation link needs to be wrapped in another element so we can hide it in the Mac App Store -->
        <span class="donate-link">
          <span>·</span>
          <a href={DONATE}>{$_('p4.donate')}</a>
        </span>
      {/if}
    </div>
    <div>
      <a href="https://docs.turbowarp.org/packager">{$_('p4.documentation')}</a>
    </div>
    <div>
      <SelectTheme />
    </div>
    <div>
      <SelectLocale />
    </div>
  </footer>
</main>