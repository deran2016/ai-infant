<template>
  <v-card
    class="mx-auto my-10"
    max-width="750"
    outlined
  >
    <v-card-text
      class="row"
    >
      <canvas
        id="paint"
        width="250"
        height="250"
      >
        지원하지 않는 브라우저입니다.
      </canvas>
      <div class="column">
        <div class="row ml-1 mt-2">
          <v-btn
            v-for="(color, key) in colorPalette"
            :key="key"
            :color="color"
            style="width: 30px; height: 30px"
            fab
            @click="setColor(color)"
          />
        </div>
        <div class="row ml-3">
          <v-btn
            v-for="(color, key) in colorPalette2"
            :key="key"
            :color="color"
            style="width: 30px; height: 30px"
            fab
            @click="setColor(color)"
          />
        </div>
        <!--
        <v-color-picker
          v-model="color"
          dot-size="25"
          canvas-height="130"
          canvas-width="200"
          swatches-max-height="200"
          hide-inputs
        />
        -->
        <div class="row">
          <v-btn
            class="mx-5 mt-2"
            color="blue"
            style="width: 100px; color: white"
            @click="newCanvas"
          >
            다시 그리기
          </v-btn>
          <v-btn
            class="mx-2"
            style="width: 50px; height: 50px"
            fab
            @click="modeDraw"
          >
            <v-icon
              color="green"
            >
              mdi-lead-pencil
            </v-icon>
          </v-btn>
          <!--
          <v-btn
            class="mx-2"
            style="width: 50px; height: 50px"
            fab
            @click="modeEraser"
          >
            <v-icon
              color="red"
            >
              mdi-eraser
            </v-icon>
          </v-btn>
          -->
        </div>
        <v-btn
          color="primary"
          :disabled="disabled"
          @click="submit"
        >
          완성하기 {{ countDown > 0 ? `(${countDown})` : '' }}
        </v-btn>
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
/* eslint-disable import/no-unresolved */
/* eslint-disable global-require */
/* eslint-disable import/no-extraneous-dependencies */
import { mapMutations } from 'vuex';

export default {
  data: () => ({
    countDown: 10,
    countUp: 0,
    painting: false,
    mode: 'draw',
    color: '#000000',
    colorPalette: [
      '#000000',
      '#333333',
      '#666666',
      '#999999',
      '#999999',
      '#FFFFFF',
    ],
    colorPalette2: [
      '#ff0000',
      '#FF7F00',
      '#FFFF00',
      '#009900',
      '#0099FF',
      '#082567',
      '#8000FF',
    ],
  }),

  computed: {
    canvas() {
      return this.$el.querySelector('#paint');
    },
    context() {
      return this.canvas.getContext('2d');
    },
    condition() {
      return this.$store.state.data.experimentType;
    },
    disabled() {
      return this.countDown > 0;
    },
  },

  mounted() {
    this.newCanvas();
    this.canvas.addEventListener('touchstart', this.startPainting);
    this.canvas.addEventListener('touchmove', this.onMouseMove);
    this.canvas.addEventListener('touchend', this.stopPainting);
    this.canvas.addEventListener('touchcancel', this.stopPainting);
    this.countDownTimer();
    this.countUpTimer();
  },

  methods: {
    ...mapMutations([
      'updateFields',
    ]),

    submit() {
      this.updateFields({ drawingTime: this.countUp });
      this.$router.push({ name: 'Summary' });
    },

    setColor(color) {
      this.color = color;
    },

    newCanvas() {
      const img = new Image();
      img.src = require('@/assets/img/나이테 나무.png');
      this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
      img.onload = () => {
        this.context.drawImage(img, 0, 0, this.canvas.width, this.canvas.height);
      };
    },

    modeDraw() {
      this.mode = 'draw';
    },

    modeEraser() {
      this.mode = 'eraser';
    },

    startPainting(event) {
      let pos = { x: event.clientX, y: event.clientY };
      if (event.touches) {
        pos = { x: event.touches[0].clientX, y: event.touches[0].clientY };
      }
      const rect = event.target.getBoundingClientRect();
      const xRel = pos.x - rect.left;
      const yRel = pos.y - rect.top;
      const x = Math.round((xRel * event.target.width) / rect.width);
      const y = Math.round((yRel * event.target.height) / rect.height);
      this.painting = true;
      this.context.beginPath();
      if (this.mode === 'eraser') {
        this.context.strokeStyle = '#ffffff';
        this.context.lineWidth = 10;
      } else if (this.mode === 'draw') {
        this.context.strokeStyle = this.color;
        this.context.lineWidth = 2;
      }
      this.context.moveTo(x, y);
    },

    stopPainting() {
      this.painting = false;
      this.context.closePath();
      this.context.save();
    },

    onMouseMove(event) {
      event.preventDefault();
      let pos = { x: event.clientX, y: event.clientY };
      if (event.touches) {
        pos = { x: event.touches[0].clientX, y: event.touches[0].clientY };
      }
      const rect = event.target.getBoundingClientRect();
      const xRel = pos.x - rect.left;
      const yRel = pos.y - rect.top;
      const x = Math.round((xRel * event.target.width) / rect.width);
      const y = Math.round((yRel * event.target.height) / rect.height);
      this.context.lineTo(x, y);
      this.context.stroke();
    },

    countDownTimer() {
      if (this.countDown > 0) {
        setTimeout(() => {
          this.countDown -= 1;
          this.countDownTimer();
        }, 1000);
      }
    },

    countUpTimer() {
      if (this.$router.history.current.name === 'Game') {
        setTimeout(() => {
          this.countUp = ((this.countUp * 10) + 1) / 10;
          this.countUpTimer();
        }, 100);
      }
    },
  },
};
</script>

<style>
.row {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.column {
  display: flex;
  flex-direction: column;
}
#paint {
  border: 1px solid #ccc;
}
body { overflow: hidden; }
body * { touch-action: none; }
</style>
