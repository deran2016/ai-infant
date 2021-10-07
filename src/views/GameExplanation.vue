<template>
  <v-card
    class="mx-auto my-10"
    max-width="750"
    outlined
  >
    <v-card-text>
      <v-btn
        v-if="!isPlaying && !isComplete"
        style="position: fixed; top: 10px;"
        fab
        @click="initAudio()"
      >
        ▶︎
      </v-btn>
      <v-row
        rows="12"
        sm="6"
      >
        <v-col>
          <div style="width: 70%; margin: 0 auto">
            <img
              :src="require('@/assets/img/나이테 나무.png')"
              style="text-align: center; width: 100%"
            />
          </div>
          <div class="text-center title">
            3살 나무 나이테는 몇 개일까?<br />
            이어서 그려볼래?
          </div>
        </v-col>
        <div style="width: 45%; margin: 0 auto">
          <img
            :src="selectImage()"
            style="text-align: center"
            :height="getHeight()"
          />
        </div>
      </v-row>
    </v-card-text>
    <v-card-actions
      style="flex-direction: row-reverse"
    >
      <v-btn
        v-if="isComplete"
        style="position: fixed; bottom: 10px; right: 30px; font-size: 30px; width: 30%; height:50px"
        color="primary"
        @click="submit"
      >
        그리러 가기
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
/* eslint-disable import/no-unresolved */
/* eslint-disable global-require */
/* eslint-disable import/no-extraneous-dependencies */
export default {
  data: () => ({
    countDown: 10,
    isPlaying: false,
    isComplete: false,
  }),

  computed: {
    condition() {
      return this.$store.state.data.experimentType;
    },
    disabled() {
      return this.countDown > 0;
    },
  },

  mounted() {
    this.initAudio();
  },

  methods: {
    submit() {
      this.$router.push({ name: 'Game' });
    },

    countDownTimer() {
      if (this.countDown > 0) {
        setTimeout(() => {
          this.countDown -= 1;
          this.countDownTimer();
        }, 1000);
      }
    },

    selectImage() {
      let image;
      if (this.condition === '1') {
        image = require('@/assets/img/3.png');
      } else if (this.condition === '2') {
        image = require('@/assets/img/유아선생님.jpg');
      } else if (this.condition === '3' || this.condition === '4') {
        image = require('@/assets/img/유아.png');
      }
      return image;
    },

    getHeight() {
      let height;
      if (this.condition === '1' || this.condition === '3') {
        height = 250;
      } else if (this.condition === '2' || this.condition === '4') {
        height = 200;
      }
      return height;
    },

    initAudio() {
      if (this.condition === '1') {
        this.playAudio('그림그리기-선생님');
      } else if (this.condition === '2') {
        this.playAudio('그림그리기-유아선생님');
      } else if (this.condition === '3' || this.condition === '4') {
        this.playAudio('그림그리기-유아');
      }
    },

    playAudio(file) {
      this.file = file;

      const audio = document.getElementById(this.file);
      try {
        audio.play();
        audio.onplaying = () => {
          this.isPlaying = true;
        };
        audio.onended = () => {
          this.isPlaying = false;
          this.isComplete = true;
        };
        audio.onerror = () => {
          this.isPlaying = false;
        };
      } catch (e) {
        this.isPlaying = false;
        console.log(e);
      } finally {
        this.isPlaying = false;
      }
    },

    stopAudio() {
      const audio = document.getElementById(this.file);
      audio.currentTime = 0;
      audio.pause();
    },
  },
};
</script>

<style>
body * { touch-action: auto; }
</style>
