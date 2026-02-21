<script>
  import {createEventDispatcher} from 'svelte';
  import {_} from '../locales/';
  import icon from './reset.svg';

  const dispatch = createEventDispatcher();

  const click = () => {
    if (confirm($_('reset.confirm'))) {
      dispatch('click');
    }
  };
</script>

<style>
  button {
    width: 36px;
    height: 36px;
    margin: 0;
    padding: 8px;
    border: none;
    background: transparent;
    cursor: pointer;
    border-radius: var(--md-sys-shape-corner-full, 9999px);
    color: var(--md-sys-color-on-surface-variant, #534342);
    transition: 
      background-color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1)),
      color var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
    display: flex;
    align-items: center;
    justify-content: center;
    outline: none;
  }
  
  button:hover {
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.08);
    color: var(--md-sys-color-primary, #BA1A1A);
  }
  
  button:active {
    background-color: rgba(var(--md-sys-color-primary-rgb, 186, 26, 26), 0.12);
  }
  
  button:focus-visible {
    outline: 2px solid var(--md-sys-color-primary, #BA1A1A);
    outline-offset: 2px;
  }
  
  img {
    width: 20px;
    height: 20px;
    opacity: 0.8;
    transition: opacity var(--md-sys-motion-duration-short2, 100ms) var(--md-sys-motion-easing-standard, cubic-bezier(0.2, 0, 0, 1));
  }
  
  button:hover img {
    opacity: 1;
  }
</style>

<button on:click={click} title={$_('reset.reset')}>
  <img
    src={icon}
    draggable={false}
    alt={$_('reset.reset')}
  >
</button>