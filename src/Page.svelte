<script lang="ts">
  import Week from "./Week.svelte";
  import Form from "./Form.svelte";
  import { weekLength } from "./utils/constants";
  import type { Settings } from "./utils/Types";

  // Constants
  const pageTitle = "Life display";

  // Main logic
  let years: number, birthDate: Date;
  $: unsetted =
    years === undefined || birthDate === undefined || birthDate === null;
  $: endDate = unsetted
    ? null
    : new Date(
        new Date(birthDate).setFullYear(birthDate.getFullYear() + years)
      );
  $: weeksAmount = unsetted ? 0 : Math.ceil((+endDate - +birthDate) / weekLength);
  $: currentWeek = unsetted
    ? 0
    : Math.ceil((+new Date() - +birthDate) / weekLength);

  // Perform form actions
  const handleShow: (data: { detail: Settings }) => void = ({ detail }) => {
    years = +detail.years;
    birthDate = detail.birthDate;
  };
</script>

<style>
  :global(body) {
    padding: 10px;
    min-width: 435px;
    box-sizing: border-box;
  }

  .app :global(.form) {
    margin-bottom: 10px;
  }

  .calendar {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    align-items: center;
  }

  .calendar :global(.calendar__item) {
    margin: 0 3px 3px 0;
  }
</style>

<svelte:head>
  <title>{pageTitle}</title>
</svelte:head>

<div class="app">
  <Form pageTitle={pageTitle} on:show={handleShow} class={'form'} />

  <section class={'calendar'}>
    {#each Array(weeksAmount) as _, i}
      <Week done={i + 1 <= currentWeek} class="calendar__item" />
    {/each}
  </section>
</div>
