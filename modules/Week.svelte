<script>
  export let done = false;
</script>

<div class={"week"} class:week--done_true={done}></div>

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
