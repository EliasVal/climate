<div class="earth">
  <div class="imgs">
    <img src="./images/earth.png" />
    <img src="./images/earth.png" />
  </div>
</div>

<style lang="scss">
  @import "../styles/vars.scss";
  .earth {
    width: $earthSize;
    height: $earthSize;

    position: absolute;
    top: calc(#{$earthSize} / -2 - #{$borderSize});

    /* CENTER EARTH */
    left: 0;
    right: 0;
    margin: 0 auto;

    /* FOR SOME REASON CSS FORGETS THAT I DO OVERFLOW HIDDEN 
      WHEN ROTATING 90DEG SO I GOTTA ROTATE IT BY 90.00001DEG */
    transform: rotateZ(90.00001deg);

    overflow: hidden;

    border-radius: 50%;
    border: 5px #000 solid;
    .imgs {
      animation: move 5s linear infinite;
      height: 100%;
      width: 200%;
      overflow: visible;
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
