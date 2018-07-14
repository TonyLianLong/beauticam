<template>
  <div id="app" class="demo-frame">
    <div class="demo-container">
      <video id="video" width="320" height="240" preload autoplay loop muted></video>
      <canvas id="canvas" width="320" height="240"></canvas>
    </div>
    <img id="left" src='./assets/ears/left.png' style="display: none" />
    <img id="right" src='./assets/ears/right.png' style="display: none" />
  </div>
</template>

<script>
import 'tracking/build/tracking'
import 'tracking/build/data/face'

// For ESLint:
/* global tracking */

const OFFSET_X = -10;
const OFFSET_Y = 0;

export default {
  name: 'app',
  components: {
  },
  mounted: () => {
    var ear_left = document.getElementById("left");
    var ear_right = document.getElementById("right");
    // TODO: wait until onload
    var canvas = document.getElementById('canvas');
    var context = canvas.getContext('2d');

      var tracker = new tracking.ObjectTracker('face');
      tracker.setInitialScale(4);
      tracker.setStepSize(2);
      tracker.setEdgesDensity(0.1);

      tracking.track('#video', tracker, { camera: true });

      tracker.on('track', function(event) {
        if (event.data.length == 0) {
          return;
        }
        context.clearRect(0, 0, canvas.width, canvas.height);

        event.data.forEach(function(rect) {
          /*context.strokeStyle = '#a64ceb';
          context.strokeRect(rect.x, rect.y, rect.width, rect.height);*/
          context.drawImage(ear_left, rect.x + OFFSET_X, rect.y + OFFSET_Y, 20, 30);
          context.drawImage(ear_right, rect.x + rect.width + OFFSET_X, rect.y + OFFSET_Y, 20, 30);

          /*context.font = '11px Helvetica';
          context.fillStyle = "#fff";
          context.fillText('x: ' + rect.x + 'px', rect.x + rect.width + 5, rect.y + 11);
          context.fillText('y: ' + rect.y + 'px', rect.x + rect.width + 5, rect.y + 22);*/
        });
      });
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.demo-container {
  position: absolute;
  top: 0;
  left: 0;
  video, canvas {
    position: absolute;
    top: 0;
    left: 0;
  }
}
</style>
