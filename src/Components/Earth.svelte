<div id="earth" class="earth">
  <div class="imgs">
    <img src="./images/earth.png" alt="" />
    <img src="./images/earth.png" alt="" />
  </div>
</div>

<style lang="scss">
  @import "../styles/vars.scss";
  .earth {
    width: 40%;
    aspect-ratio: 1 / 1;
    /* CENTER EARTH */
    margin: 0 auto;

    z-index: 10;

    box-sizing: border-box;
    /* FOR SOME REASON CSS FORGETS THAT I DO OVERFLOW HIDDEN 
      WHEN ROTATING 90DEG SO I GOTTA ROTATE IT BY 90.00001DEG */
    transform: rotateZ(90.00001deg);

    overflow: hidden;

    border-radius: 50%;
    border: 5px #000 solid;
    .imgs {
      animation: move 5s linear infinite;
      height: 100%;
      width: calc(100% * 2);
      display: flex;
    }
  }

  @keyframes move {
    from {
      transform: translateX(0);
    }
    to {
      transform: translateX(-50%);
    }
  }
</style>
