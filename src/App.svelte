<script lang="ts">
  import Earth from "./Components/Earth.svelte";
</script>

<h1 style="text-align: center; padding-top: 2rem">
  The Earth is being cooked, while we are in it.
</h1>
<div class="shawarma">
  <Earth />
</div>

<style lang="scss">
  @import "../styles/vars.scss";

  .shawarma {
    width: 50%;
    border: $borderSize #fa48af solid;
    height: 250px;
    border-bottom: none;
    position: fixed;

    inset: 0;
    margin: auto;
  }
</style>
