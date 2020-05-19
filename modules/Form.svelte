<script>
  import {slide} from 'svelte/transition';
  import queryString from 'querystring';
  import {createEventDispatcher, onMount} from 'svelte';
  import {path} from '../js/constants';

  // Events
  const dispatch = createEventDispatcher();
  const dispatchShow = detail => dispatch('show', detail);
  onMount(() => dispatchShow({
    birthDate: dateIncorrect ? null : birthDate,
    years,
  }));


  // Reg exps
  const regExps = {
    birthDate: /\d{1,2}\.\d{1,2}\.\d{4}/,
  };


  // props
  export let pageTitle;


  // Query params
  let params = queryString.parse(window.location.search.slice(1));


  // Birth date
  let birthDateSource = params.date || '';
  let birthDateIsFocused = false;

  $: dateIncorrect = !regExps.birthDate.test(birthDateSource);
  $: inputIncorrect = birthDateSource.length && dateIncorrect;
  $: birthDate = new Date(birthDateSource.split('.').reverse().join('-'));

  const handleFocusIn = () => birthDateIsFocused = true;
  const handleFocusOut = () => birthDateIsFocused = false;


  // Years
  let years = params.years || 1;


  // Form action
  const handleSubmit = () => {
    if (window.location.protocol !== 'file:') {
      window.history.pushState({pageTitle}, "", `${path}?date=${birthDateSource}&years=${years}`);
    }

    dispatchShow({birthDate, years});
  };
</script>

<form class="data" on:submit|preventDefault={handleSubmit}>
  <label class="label">
    <h3>Birth date:</h3>
    <input type="text" on:focusin={handleFocusIn} on:focusout={handleFocusOut} bind:value={birthDateSource} class:input--correct_false={inputIncorrect}>
    {#if birthDateIsFocused}
      <p transition:slide>Format: dd.mm.yyyy</p>
    {/if}
  </label>

  <label class="label">
    <h3>Years to show:</h3>
    <input type="number" min={0} bind:value={years}>
  </label>

  <button type="submit" class="button" disabled={dateIncorrect}>Show me!</button>
</form>

<style>
  .label {
    display: block;
  }

  .button {
    margin-top: 10px;
    display: block;
  }

  .input--correct_false {
    border-color: tomato;
  }
</style>
