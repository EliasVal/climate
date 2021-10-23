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
      border-radius: 5px;
      .progressBar {
        border-radius: 5px;
        position: relative;
      }
    }
  }
</style>
