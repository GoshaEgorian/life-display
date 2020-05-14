<script>
  import Week from './Week.svelte';
  import Form from './Form.svelte';

  const pageTitle = 'Life display';
  const weeksAmount = 59;
  const currentWeek = 31;

  let years, birthDate;
  const testLog = ({detail}) => console.log('Bang!');
  // window.history.pushState({pageTitle}, "", `${window.location.origin}?test=${rand}`);
</script>

<svelte:head>
  <link rel="stylesheet" href="./index.css">
  <title>{pageTitle}</title>
</svelte:head>

<Form pageTitle on:show={testLog}/>

<section class={"calendar"}>
  {#each Array(weeksAmount) as _, i}
    <Week done={i+1 <= currentWeek} />
  {/each}
</section>

<style>
  @import "../node_modules/normalize.css";

  .calendar {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
  }
</style>
