<script lang="ts">
  import { onMount } from "svelte";

  import Earth from "./Components/Earth.svelte";
  import F1 from "./Components/Factories/F1.svelte";
  import F2 from "./Components/Factories/F2.svelte";
  import F3 from "./Components/Factories/F3.svelte";
  import ProgressBar from "./Components/ProgressBar.svelte";
  import StickSide from "./Components/Stick-Side.svelte";
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
  });

  async function Spawning() {
    const point = spawnpoints[GenRandom(spawnpoints.length)];
    const div = document.createElement("span");
    const size = GenRandom(maxBubbleSize, minBubbleSize);
    const lifetime =
      window.innerWidth > 615 ? GenRandom(5, 10) : GenRandom(3, 5);

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

    setTimeout(Spawning, 50);
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
<svelte:body on:scroll={res} />
<main style="height: 100vh; display: flex; flex-direction: column; gap: 5rem;">
  <div class="shawarma">
    <div class="sticks" id="sticks">
      <img src="./images/stick-t.svg" id="t" alt="" />
      <div class="sides">
        <div class="stick">
          <StickSide />
        </div>
        <div class="stick">
          <StickSide />
        </div>
      </div>
    </div>
    <Earth />

    <div class="factoryContainer">
      <div style="left: 50%; transform: translateX(-75%);">
        <F3 />
      </div>
      <div style="left: 50%; transform: translateX(-10%);">
        <F2 />
      </div>
      <div style="left: 50%; transform: translateX(-50%);">
        <F1 />
      </div>
    </div>
  </div>
  <ProgressBar />
</main>

<style lang="scss" global>
  @import "./styles/vars.scss";
  .shawarma {
    width: 50%;
    margin: 20px auto 0 auto;
    position: relative;
  }

  .factoryContainer {
    z-index: 3;
    margin: 60px 0 0 0;
    width: 100%;
    position: relative;
    height: 13rem;

    svg {
      width: 15rem;
    }
    div {
      position: absolute;
      bottom: 0;
    }
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
      z-index: 1;
    }

    .sides {
      height: 100%;
      width: 110%;
      display: flex;
      justify-content: space-between;
      position: absolute;
      top: -5%;
      left: -5%;

      .stick {
        position: relative;
        .stick-main {
          z-index: 2;
          position: relative;
        }
        .stick-part {
          z-index: 0;
          position: absolute;
          top: 0;
        }
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
    .factoryContainer svg {
      width: 8rem;
    }
  }

  img,
  .unselectalble {
    user-select: none;
  }
</style>
