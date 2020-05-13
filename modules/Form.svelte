<script>
  import {slide} from 'svelte/transition';

  const regExps = {
    birthDate: /\d{2}\.\d{2}\.\d{4}/,
  };

  // Birth date
  let birthDateSource = '';
  let birthDateIsFocused = false;

  $: dateIncorrect = birthDateSource.length && !regExps.birthDate.test(birthDateSource);
  $: dateParsed = birthDateSource.split('.');
  $: birthDate = new Date(`${dateParsed[1]} ${dateParsed[0]}, ${dateParsed[2]}`);

  let handleFocusIn = () => birthDateIsFocused = true;
  let handleFocusOut = () => birthDateIsFocused = false;


  // Years
  let years = 1;
</script>

<form class="data">
  <label>
    <h3>Birth date:</h3>
    <input type="text" on:focusin={handleFocusIn} on:focusout={handleFocusOut} bind:value={birthDateSource} class:incorrect={dateIncorrect}>
    {#if birthDateIsFocused}
      <p transition:slide>Format: dd.mm.yyyy</p>
    {/if}
  </label>

  <label>
    <h3>Years to show:</h3>
    <input type="number" min={0} bind:value={years}>
  </label>
</form>

<style>
  .incorrect {
    border-color: tomato;
  }
</style>
