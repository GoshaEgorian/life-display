<script>
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
  let className = '';
  export {className as class};


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
      window.history.pushState({pageTitle}, "", `${path}?${queryString.encode({date: birthDateSource, years})}`);
    }

    dispatchShow({birthDate, years});
  };
</script>

<form class="data {className}" on:submit|preventDefault={handleSubmit}>
  <label class="data__item">
    <h3 class="data__title">Birth date:</h3>
    <input
      type="text"
      on:focusin={handleFocusIn}
      on:focusout={handleFocusOut}
      bind:value={birthDateSource}
      class:input--correct_false={inputIncorrect}
      placeholder="dd.mm.yyyy"
    >
  </label>

  <label class="data__item">
    <h3 class="data__title">Years to show:</h3>
    <input type="number" min={0}>
  </label>

  <button type="submit" class="button" disabled={dateIncorrect}>Show me!</button>
</form>

<style>
  .data {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .data__item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  .data__title {
    margin: 0 5px 0 0;
  }

  .input--correct_false {
    border: 2px inset tomato;
    border-radius: 0.35em;
  }

  @media (min-width: 950px) {
    .data {
      margin: 0 auto;
      flex-direction: row;
      justify-content: space-around;
      max-width: 950px;
    }

    .data__item {
      margin-bottom: 0;
    }
  }
</style>
