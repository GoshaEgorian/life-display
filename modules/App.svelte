<script>
  import Week from './Week.svelte';
  import Form from './Form.svelte';
  import {weekLength} from '../js/constants';

  // Constants
  const pageTitle = 'Life display';


  // Main logic
  let years, birthDate;
  $: unsetted = years === undefined || birthDate === undefined || birthDate === null;
  $: endDate = unsetted ? null : new Date(new Date(birthDate).setFullYear(birthDate.getFullYear() + years));
  $: weeksAmount = unsetted ? 0 : Math.ceil((endDate - birthDate) / weekLength);
  $: currentWeek = unsetted ? 0 : Math.ceil((new Date() - birthDate) / weekLength);


  // Perform form actions
  const handleShow = ({detail}) => {
    years = +detail.years;
    birthDate = detail.birthDate;
  };
</script>

<svelte:head>
  <link rel="stylesheet" href="./index.css">
  <title>{pageTitle}</title>
</svelte:head>

<Form pageTitle on:show={handleShow}/>

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
    justify-content: flex-start;
    align-items: center;
    gap: 3px;
  }
</style>
