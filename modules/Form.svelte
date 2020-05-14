<script>
  import {slide} from 'svelte/transition';
  import queryString from 'query-string';
  import {createEventDispatcher} from 'svelte';

  // Events
  const dispatch = createEventDispatcher();
  const dispatchShow = detail => dispatch('show', detail);


  // Reg exps
  const regExps = {
    birthDate: /\d{2}\.\d{2}\.\d{4}/,
  };


  // some of props
  export let pageTitle;


  // Query params
  let params = queryString.parse(window.location.search);


  // Birth date
  let birthDateSource = params.date || '';
  let birthDateIsFocused = false;

  $: dateIncorrect = birthDateSource.length && !regExps.birthDate.test(birthDateSource);
  $: dateParsed = birthDateSource.split('.');
  $: birthDate = new Date(`${dateParsed[1]} ${dateParsed[0]}, ${dateParsed[2]}`);

  const handleFocusIn = () => birthDateIsFocused = true;
  const handleFocusOut = () => birthDateIsFocused = false;


  // Years
  let years = params.years || 1;


  // Form action
  const handleSubmit = () => {
    window.history.pushState({pageTitle}, "", `${window.location.origin}?date=${birthDateSource}&years=${years}`);
    dispatchShow({birthDate, years});
  };
</script>

<form class="data" on:submit|preventDefault={handleSubmit}>
  <label class="label">
    <h3>Birth date:</h3>
    <input type="text" on:focusin={handleFocusIn} on:focusout={handleFocusOut} bind:value={birthDateSource} class:incorrect={dateIncorrect}>
    {#if birthDateIsFocused}
      <p transition:slide>Format: dd.mm.yyyy</p>
    {/if}
  </label>

  <label class="label">
    <h3>Years to show:</h3>
    <input type="number" min={0} bind:value={years}>
  </label>

  <button type="submit" class="button">Show me!</button>
</form>

<style>
  .label {
    display: block;
  }

  .button {
    margin-top: 10px;
    display: block;
  }

  .incorrect {
    border-color: tomato;
  }
</style>
