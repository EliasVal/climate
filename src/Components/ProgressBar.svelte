<script>
  import Timer from "./Timer.svelte";
  const startDate = new Date("Jan 1 1980");
  const endDate = new Date("Jul 21 2029");

  const diff = 100 / (endDate.getTime() - startDate.getTime());

  let percentage;
  calcPercentage();
  setInterval(calcPercentage, 1000);
  function calcPercentage() {
    percentage = Math.abs(Date.now() - startDate.getTime()) * diff;
  }

  let v = 0;
  setInterval(() => {
    document.documentElement.style.setProperty("--offset", `${v}px`);
    v += 0.2;
  }, 1);
</script>

<div class="main">
  <div>
    <h1 style="font-size: 1em;">
      Time left until climate change is irreversible:
    </h1>
    <Timer />
  </div>
  <div class="progress">
    <div class="progressBg">
      <div
        class="progressBar"
        style="width: {percentage}%; background-color: red;"
      />
    </div>
  </div>
</div>

<style lang="scss">
  :root {
    --offset: 0px;
  }

  .main {
    padding: 2rem 0;
    max-width: 1100px;
    margin: 0 auto;
    width: 100%;
  }

  .progress {
    height: 3rem;
    div {
      height: 100%;
    }
    border-radius: 10px;
    box-sizing: border-box;
    border: 5px #000 solid;
    background-color: black;

    .progressBg {
      background-color: white;
      position: relative;
      border-radius: 5px;

      .progressBar {
        border-radius: 5px;
        position: relative;
        z-index: 1;
      }
    }
  }

  .progressBar::before {
    content: "";
    position: absolute;
    top: 0px;
    right: 0px;
    bottom: 0px;
    left: 0px;
    background-image: url(/images/loadingLine.svg);
    background-repeat: repeat-x;
    background-position-x: var(--offset);
    opacity: 0.2;
    z-index: 0;
  }

  @media only screen and (max-width: 1150px) {
    .main {
      width: auto;
      margin: 0 30px;
    }
  }
</style>
