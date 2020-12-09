<script lang="ts">
  let done: boolean, className = '';
  export {done, className as class};
</script>

<div class="week {className}" class:week--done_true={done}></div>

<style>
  .week {
    width: 13px;
    height: 13px;
    border: 1px solid grey;
  }
  .week--done_true {
    background-color: grey;
  }
</style>
