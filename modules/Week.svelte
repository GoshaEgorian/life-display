<script>
  export let done = false;
</script>

<div class={"week"} class:done></div>

<style>
  .week {
    width: 13px;
    height: 13px;
    border: 1px solid grey;
  }
  .done {
    background-color: grey;
  }
</style>
