<script lang="ts">
  import { onMount } from "svelte";

  import Earth from "./Components/Earth.svelte";
  import { GenRandom } from "./ts/utils";

  const minBubbleSize = 5,
    maxBubbleSize = 15;

  let spawnpoints: HTMLCollectionOf<Element>;
  onMount(() => {
    spawnpoints = document.getElementsByClassName("spawn");
    console.log(spawnpoints);
    Spawning();
  });

  function Spawning() {
    let point = spawnpoints[GenRandom(spawnpoints.length)];

    let div = document.createElement("div");

    div.style.width = `${GenRandom(maxBubbleSize, minBubbleSize)}px`;
    div.style.height = div.style.width;

    div.style.backgroundColor = "#000";
    div.style.opacity = div.style.borderRadius = `${GenRandom(80, 30)}%`;

    div.classList.add("smoke");

    point.appendChild(div);

    setTimeout(Spawning, 1000);
  }
</script>

<h1 style="text-align: center; padding-top: 2rem">
  The Earth is being cooked, while we are in it.
</h1>
<div class="shawarma">
  <Earth />
  <div class="factoryContainer">
    <div class="spawnpoints">
      <span class="spawn" />
      <span class="spawn" />
      <span class="spawn" />
      <span class="spawn" />
      <span class="spawn" />
    </div>
  </div>
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

  .factoryContainer {
    margin-top: calc(#{$earthSize} - 50px);
    .spawnpoints {
      display: flex;
      justify-content: space-between;
      .spawn {
        width: 1px;
      }
    }
  }

  @keyframes grow {
    from {
      transform: scale(0.3);
    }
    to {
      transform: scale(1);
    }
  }

  :global(.smoke) {
    animation: grow 0.5s linear forwards;
  }
</style>
