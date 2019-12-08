<script>
  import Week from './Week.svelte';

  const weeksAmount = 59;
  const currentWeek = 31;
</script>

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
