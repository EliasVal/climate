<script lang="ts">
  import { onMount } from "svelte";

  import Earth from "./Components/Earth.svelte";
  import F1 from "./Components/Factories/F1.svelte";
  import ProgressBar from "./Components/ProgressBar.svelte";
  import { GenRandom } from "./ts/utils";

  const minBubbleSize = 20,
    maxBubbleSize = 35;

  let spawnpoints: Array<{
    spawn: HTMLElement;
    creator: HTMLElement | Element;
  }> = [];

  onMount(() => {
    const cSpawns = document.getElementsByClassName("createSpawn");
    for (const spawn of cSpawns) {
      const span = document.createElement("span");
      span.style.position = "absolute";
      span.style.left =
        window.pageXOffset + spawn.getBoundingClientRect().left - 10 + "px";
      span.style.top =
        window.pageYOffset + spawn.getBoundingClientRect().top + "px";

      span.classList.add("spawn");
      spawnpoints.push({ spawn: span, creator: spawn });
      document.querySelector("main").appendChild(span);
    }
    Spawning();
    console.log(spawnpoints);
  });

  async function Spawning() {
    const point = spawnpoints[GenRandom(spawnpoints.length)];
    const div = document.createElement("span");
    const size = GenRandom(maxBubbleSize, minBubbleSize);
    const lifetime = GenRandom(5, 10);

    div.style.height = `${size}px`;
    div.style.width = div.style.height;
    div.style.zIndex = GenRandom(3).toString();

    let color = "#";
    if (point.spawn.hasAttribute("color"))
      color = point.spawn.getAttribute("color");
    // If two colors were provided
    else if (point.spawn.hasAttribute("colors")) {
      // split colors provided into array
      const colors = point.spawn
        .getAttribute("colors")
        .split(/\s*;\s*/)
        .slice(0, 2);
      console.log(colors);

      const color1Vals = [];
      const color2Vals = [];

      // convert each set of 2 Hex numbers to Ints
      colors[0]
        .replace("#", "")
        .match(/.{1,2}/g)
        .map((v) => {
          color1Vals.push(parseInt(v, 16));
        });

      colors[1]
        .replace("#", "")
        .match(/.{1,2}/g)
        .map((v) => {
          color2Vals.push(parseInt(v, 16));
        });

      // Iterate through each number and generate one between them.
      color1Vals.map((v, i) => {
        let gNum;
        if (v < color2Vals[i]) gNum = GenRandom(color2Vals[i], v);
        else if (v > color2Vals[i]) gNum = GenRandom(v, color2Vals[i]);
        else gNum = v;
        color += gNum.toString(16);
      });
    } else color = "#000";

    div.style.backgroundColor = color;
    div.style.opacity = `${GenRandom(80, 30) / 100}`;
    div.style.borderRadius = "50%";
    div.style.position = "absolute";

    point.spawn.appendChild(div);
    FloatDiv(div, 0);
    setTimeout(async () => {
      div.style.animation = "fadeOut 2s ease-out forwards";
      setTimeout(() => {
        point.spawn.removeChild(div);
      }, 2000);
    }, lifetime * 1000);

    setTimeout(Spawning, 20);
  }

  async function FloatDiv(
    div: HTMLSpanElement,
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

  onMount(res);
  function res() {
    document.getElementById("sticks").style.top = `${
      document.getElementById("earth").clientHeight / 2
    }px`;

    for (let i = 0; i < spawnpoints.length; i++) {
      spawnpoints[i].spawn.style.left =
        window.pageXOffset +
        spawnpoints[i].creator.getBoundingClientRect().x -
        10 +
        "px";

      spawnpoints[i].spawn.style.top =
        window.pageYOffset +
        spawnpoints[i].creator.getBoundingClientRect().top +
        "px";
    }
  }
</script>

<svelte:window on:resize={res} />
<main style="height: 100vh; display: flex; flex-direction: column">
  <h1 style="text-align: center; padding-top: 2rem">
    The Earth is being cooked, while we are in it.
  </h1>
  <div class="shawarma">
    <div class="sticks" id="sticks">
      <img src="./images/stick-t.png" id="t" alt="" />
      <div class="sides">
        <img src="./images/stick.png" id="l" alt="" />
        <img src="./images/stick.png" id="r" alt="" />
      </div>
    </div>
    <Earth />

    <div class="factoryContainer">
      <F1 />
    </div>
  </div>
  <ProgressBar />
</main>

<style lang="scss" global>
  @import "./styles/vars.scss";
  svg {
    z-index: 3;
    width: 15rem;
  }
  .shawarma {
    width: 50%;
    margin: auto;
    position: relative;
  }

  .factoryContainer {
    margin: 50px auto 0 auto;
    width: fit-content;
  }

  .sticks {
    position: absolute;
    width: 100%;
    bottom: 25%;
    bottom: 0;
    display: flex;
    flex-direction: column;

    #t {
      width: 100%;
      transform: scaleX(1.2);
      height: 5px;
    }

    .sides {
      height: 100%;
      display: flex;
      justify-content: space-between;
      #r {
        right: 0;
      }

      #r,
      #l {
        width: 5px;
        height: 100%;
        transform: scale(1.1);
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

  @media only screen and (max-width: 1024px) {
    .shawarma {
      width: 75%;
    }
  }

  img,
  .unselectalble {
    user-select: none;
  }
</style>
