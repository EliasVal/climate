<script>
  // The data/time we want to countdown to
  const destDate = new Date("Jul 21, 2029").getTime();

  let years = 0;
  let days = 0;
  let hours = 0;
  let minutes = 0;
  let seconds = 0;

  CalcTime();
  setInterval(CalcTime, 1000);

  function CalcTime() {
    let now = new Date().getTime();
    let timeleft = destDate - now;

    // Calculating the days, hours, minutes and seconds left
    seconds = Math.floor((timeleft % (1000 * 60)) / 1000);
    minutes = Math.floor((timeleft % (1000 * 60 * 60)) / (1000 * 60));
    hours = Math.floor((timeleft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    days = Math.floor((timeleft / (1000 * 60 * 60 * 24)) % 365);
    years = Math.floor(timeleft / (1000 * 60 * 60 * 24 * 365));
  }
</script>

<div>
  <h1>{years}<span class="timeName">years</span></h1>
  <h1>{days}<span class="timeName">days</span></h1>
  <h1>
    {hours}<span class="timeName">hours</span>
    {minutes}<span class="timeName">minutes</span>
    {seconds}<span class="timeName">seconds</span>
  </h1>
</div>

<style lang="scss">
  .timeName {
    font-size: medium;
  }
</style>
