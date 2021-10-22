<script lang="ts">
  import { onMount } from "svelte";

  import Earth from "./Components/Earth.svelte";
  import { GenRandom } from "./ts/utils";

  const minBubbleSize = 20,
    maxBubbleSize = 35;

  let spawnpoints: HTMLCollectionOf<Element>;
  onMount(() => {
    spawnpoints = document.getElementsByClassName("spawn");
    Spawning();
  });

  async function Spawning() {
    const point = spawnpoints[GenRandom(spawnpoints.length)];
    const div = document.createElement("div");
    const size = GenRandom(maxBubbleSize, minBubbleSize);
    const lifetime = GenRandom(5, 10);

    div.style.height = `${size}px`;
    div.style.width = div.style.height;

    div.style.backgroundColor = "#000";
    div.style.opacity = `${GenRandom(80, 30) / 100}`;
    div.style.borderRadius = "50%";
    div.style.position = "absolute";

    point.appendChild(div);
    FloatDiv(div, 0);
    setTimeout(async () => {
      div.style.animation = "fadeOut 2s ease-out forwards";
      setTimeout(() => {
        point.removeChild(div);
      }, 2000);
    }, lifetime * 1000);

    setTimeout(Spawning, 20);
  }

  async function FloatDiv(
    div: HTMLDivElement,
    ypos,
    size = 0,
    desxpos = 0,
    xpos = 0
  ) {
    ypos++;
    if (size < 1) size += 0.01;

    if (desxpos == ~~xpos) desxpos = GenRandom(10, -10);
    xpos += desxpos < xpos ? -0.3 : 0.3;

    div.style.transform = `translateY(${-ypos}px) translateX(${xpos}px) scale(${size})`;
    setTimeout(async () => FloatDiv(div, ypos, size, desxpos, xpos), 20);
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

<style lang="scss" global>
  @import "./styles/vars.scss";

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

  @keyframes fadeOut {
    to {
      opacity: 0;
    }
  }
</style>
