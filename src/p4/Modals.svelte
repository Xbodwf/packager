<script>
  import Section from './Section.svelte';
  import Button from './Button.svelte';
  import {CannotAccessProjectError, OutdatedPackagerError, UnknownNetworkError, UserError} from '../common/errors';
  import {error} from './stores';
  import {FEEDBACK_PRIMARY} from '../packager/brand';
  import {_} from '../locales/';
  import ComplexMessage from './ComplexMessage.svelte';

  export let modalVisible;
  let modalElement;
  let initiallyFocusedElement;

  const getAllFocusableElements = () => Array.from(document.querySelectorAll('a, button, input, select, summary, textarea'))
    .filter((i) => !modalElement || !modalElement.contains(i));

  $: {
    modalVisible = !!$error;
    if ($error) {
      console.error($error);
      document.body.setAttribute('p4-modal-visible', '');
      initiallyFocusedElement = document.activeElement;
      getAllFocusableElements().forEach((i) => {
        i.setAttribute('p4-old-tabIndex', i.tabIndex);
        i.tabIndex = -1;
      });
    } else {
      document.body.removeAttribute('p4-modal-visible');
      getAllFocusableElements().forEach((i) => {
        if (i.hasAttribute('p4-old-tabIndex')) {
          i.tabIndex = i.getAttribute('p4-old-tabIndex');
          i.removeAttribute('p4-old-tabIndex');
        }
      });
      if (initiallyFocusedElement) {
        initiallyFocusedElement.focus();
      }
    }
  }

  $: if (modalElement) {
    const button = modalElement.querySelector('button');
    if (button) {
      button.focus();
    }
  }

  const closeModal = () => {
    $error = null;
  };
  const onKeyDown = (e) => {
    if (e.key === 'Escape') {
      closeModal();
    }
  };

  const refresh = () => location.reload();
</script>

<style>
  :global([p4-modal-visible]) {
    overflow: hidden;
  }
  
  .modal-scrim {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 20;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: var(--md-sys-color-scrim, rgba(0, 0, 0, 0.32));
    word-break: break-word;
    padding: 16px;
    animation: fade-in var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
  }
  
  @keyframes fade-in {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  
  .dialog {
    background-color: var(--md-sys-color-surface-container-high, #F9EBEA);
    border-radius: var(--md-comp-dialog-container-shape, 28px);
    padding: 24px;
    max-width: 560px;
    width: 100%;
    box-shadow: var(--md-sys-elevation-level3, 0px 4px 8px 3px rgba(0, 0, 0, 0.15), 0px 1px 3px rgba(0, 0, 0, 0.3));
    animation: dialog-enter var(--md-sys-motion-duration-medium1, 250ms) var(--md-sys-motion-easing-emphasized-decelerate, cubic-bezier(0.05, 0.7, 0.1, 1));
  }
  
  @keyframes dialog-enter {
    from {
      opacity: 0;
      transform: scale(0.9);
    }
    to {
      opacity: 1;
      transform: scale(1);
    }
  }
  
  .dialog h2 {
    font-family: var(--md-ref-typeface-brand);
    font-size: var(--md-sys-typescale-headline-small, 24px);
    font-weight: var(--md-sys-typescale-weight-regular, 400);
    color: var(--md-sys-color-on-surface, #201A1A);
    margin: 0 0 16px 0;
  }
  
  .dialog p {
    font-family: var(--md-ref-typeface-plain);
    font-size: var(--md-sys-typescale-body-large, 16px);
    color: var(--md-sys-color-on-surface-variant, #534342);
    margin: 8px 0;
    line-height: 1.5;
  }
  
  .dialog-actions {
    display: flex;
    justify-content: flex-end;
    gap: 8px;
    margin-top: 24px;
    padding-top: 16px;
  }
  
  .technical {
    font-family: 'Roboto Mono', monospace;
    font-size: var(--md-sys-typescale-body-small, 12px);
    background-color: var(--md-sys-color-surface-container-highest, #F3E5E4);
    padding: 12px;
    border-radius: var(--md-sys-shape-corner-small, 8px);
    color: var(--md-sys-color-on-surface, #201A1A);
    overflow-x: auto;
    white-space: pre-wrap;
    word-break: break-word;
  }
</style>

<svelte:window on:keydown={onKeyDown} />

{#if modalVisible}
  <!-- All prompts can be closed using buttons in the modal, so we can ignore this warning -->
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div class="modal-scrim" on:click|self={closeModal} bind:this={modalElement}>
    <div class="dialog" role="dialog" aria-modal="true">
      <h2>{$_('p4.error')}</h2>
      {#if $error instanceof UserError}
        <p>{$error.message}</p>
        <div class="dialog-actions">
          <Button on:click={closeModal} text={$_('p4.close')} />
        </div>
      {:else if $error instanceof UnknownNetworkError}
        <p>
          <ComplexMessage
            message={$_('p4.networkError')}
            values={{
              url: {
                text: $error.url,
                href: $error.url,
                newTab: true
              }
            }}
          />
        </p>
        <div class="dialog-actions">
          <Button on:click={closeModal} text={$_('p4.close')} />
        </div>
      {:else if $error instanceof OutdatedPackagerError}
        <p>{$_('p4.outdated')}</p>
        <p class="technical">{$error}</p>
        <div class="dialog-actions">
          <Button secondary on:click={closeModal} text={$_('p4.close')} />
          <Button on:click={refresh} text={$_('p4.refresh')} />
        </div>
      {:else if $error instanceof CannotAccessProjectError}
        <p>
          {$_('p4.cannotAccessProject')}
        </p>
        <p>
          {$_('select.unsharedProjects')}
        </p>
        <p>
          <ComplexMessage
            message={$_('select.unsharedProjectsMore')}
            values={{
              link: {
                text: 'https://docs.turbowarp.org/unshared-projects',
                href: 'https://docs.turbowarp.org/unshared-projects',
                newTab: true
              }
            }}
          />
        </p>
        <p>
          {$_('p4.cannotAccessProjectCaching')}
        </p>
        <div class="dialog-actions">
          <Button on:click={closeModal} text={$_('p4.close')} />
        </div>
      {:else}
        <p>{$_('p4.errorMessage').replace('{error}', $error)}</p>
        <div class="dialog-actions">
          <Button on:click={closeModal} text={$_('p4.close')} />
          <a href={FEEDBACK_PRIMARY.link}>{$_('p4.reportBug')}</a>
        </div>
      {/if}
    </div>
  </div>
{/if}