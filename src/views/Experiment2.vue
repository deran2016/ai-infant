<template>
  <v-card
    class="mx-auto"
    max-width="95vw"
    height="95vh"
    outlined
  >
    <v-card-text
      style="justify-content: center; text-align: center"
    >
      <img
        :src="img"
        height="230"
      />
    </v-card-text>

    <v-card-actions>
      <v-btn
        v-if="!isPlaying && !isComplete"
        fab
        @click="initAudio()"
      >
        ▶︎
      </v-btn>
      <v-btn
        v-if="isComplete"
        outlined
        rounded
        style="width: 20%; font-size: 30px; padding: 30px; margin-left: 120px"
        color="primary"
        @click="submit"
      >
        <img
          :src="require('@/assets/img/다음.png')"
          height="60"
        />
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
/* eslint-disable import/no-unresolved */
/* eslint-disable import/no-extraneous-dependencies */
/* eslint-disable global-require */

export default {
  data: () => ({
    file: '',
    isPlaying: false,
    isComplete: false,
    img: require('@/assets/나이테/1/009.png'),
    imgs: [[
      require('@/assets/나이테/1/001.png'),
      require('@/assets/나이테/1/002.png'),
      require('@/assets/나이테/1/003.png'),
      require('@/assets/나이테/1/004.png'),
      require('@/assets/나이테/1/005.png'),
      require('@/assets/나이테/1/006.png'),
      require('@/assets/나이테/1/007.png'),
      require('@/assets/나이테/1/008.png'),
      require('@/assets/나이테/1/009.png'),
      require('@/assets/나이테/1/010.png'),
    ], [
      require('@/assets/나이테/2/001.png'),
      require('@/assets/나이테/2/002.png'),
      require('@/assets/나이테/2/003.png'),
      require('@/assets/나이테/2/004.png'),
      require('@/assets/나이테/2/005.png'),
      require('@/assets/나이테/2/006.png'),
      require('@/assets/나이테/2/007.png'),
      require('@/assets/나이테/2/008.png'),
      require('@/assets/나이테/2/009.png'),
      require('@/assets/나이테/2/010.png'),
    ]],
  }),

  computed: {
    condition() {
      return this.$store.state.data.experimentType;
    },
  },

  mounted() {
    this.preloadImg();
    this.initImg();
    this.initAudio();
  },

  beforeDestroy() {
    this.stopAudio();
  },

  methods: {
    submit() {
      this.$router.push({ name: 'Experiment3' });
    },

    initImg() {
      if (this.condition === '1') {
        this.img = require('@/assets/나이테/1/009.png');
      } else if (this.condition === '2') {
        this.img = require('@/assets/나이테/2/009.png');
      } else if (this.condition === '3') {
        this.img = require('@/assets/나이테/1/009.png');
      } else if (this.condition === '4') {
        this.img = require('@/assets/나이테/2/009.png');
      }
    },

    initAudio() {
      if (this.condition === '1') {
        this.playAudio('나이테-선생님');
      } else if (this.condition === '2') {
        this.playAudio('나이테-선생님');
      } else if (this.condition === '3') {
        this.playAudio('나이테-선생님');
      } else if (this.condition === '4') {
        this.playAudio('나이테-선생님');
      }
    },

    preloadImg() {
      for (let i = 0; i < this.imgs[this.condition - 1].length; i += 1) {
        const img = new Image();
        img.src = this.imgs[this.condition - 1][i];
      }
      console.log('preloaded');
    },

    playAudio(file) {
      this.file = file;

      const audio = document.getElementById(this.file);
      try {
        audio.play();
        audio.onplaying = () => {
          this.isPlaying = true;
          this.checkAudioTime(audio);
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

    checkAudioTime(audio) {
      const { currentTime } = audio;
      if (this.isPlaying) {
        setTimeout(() => {
          if (this.condition === '1') {
            if (currentTime > 0 && currentTime < 4) {
              this.img = require('@/assets/나이테/1/009.png');
            } else if (currentTime > 4 && currentTime < 16) {
              this.img = require('@/assets/나이테/1/001.png');
            } else if (currentTime > 16 && currentTime < 24) {
              this.img = require('@/assets/나이테/1/002.png');
            } else if (currentTime > 24 && currentTime < 36) {
              this.img = require('@/assets/나이테/1/003.png');
            } else if (currentTime > 36 && currentTime < 50) {
              this.img = require('@/assets/나이테/1/004.png');
            } else if (currentTime > 50 && currentTime < 58) {
              this.img = require('@/assets/나이테/1/005.png');
            } else if (currentTime > 58 && currentTime < 62) {
              this.img = require('@/assets/나이테/1/006.png');
            } else if (currentTime > 62 && currentTime < 70) {
              this.img = require('@/assets/나이테/1/007.png');
            } else if (currentTime > 70 && currentTime < 79) {
              this.img = require('@/assets/나이테/1/008.png');
            } else if (currentTime > 79 && currentTime < 82) {
              this.img = require('@/assets/나이테/1/010.png');
            } else if (currentTime > 82) {
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 4) {
              this.img = require('@/assets/나이테/2/009.png');
            } else if (currentTime > 4 && currentTime < 15) {
              this.img = require('@/assets/나이테/2/001.png');
            } else if (currentTime > 15 && currentTime < 24) {
              this.img = require('@/assets/나이테/2/002.png');
            } else if (currentTime > 24 && currentTime < 37) {
              this.img = require('@/assets/나이테/2/003.png');
            } else if (currentTime > 37 && currentTime < 45) {
              this.img = require('@/assets/나이테/2/004.png');
            } else if (currentTime > 45 && currentTime < 53) {
              this.img = require('@/assets/나이테/2/005.png');
            } else if (currentTime > 53 && currentTime < 59) {
              this.img = require('@/assets/나이테/2/006.png');
            } else if (currentTime > 59 && currentTime < 65) {
              this.img = require('@/assets/나이테/2/007.png');
            } else if (currentTime > 65 && currentTime < 75) {
              this.img = require('@/assets/나이테/2/008.png');
            } else if (currentTime > 75 && currentTime < 82) {
              this.img = require('@/assets/나이테/2/010.png');
            } else if (currentTime > 82) {
              this.isComplete = true;
            }
          }
          this.checkAudioTime(audio);
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
.intro_img {
  text-align: center;
}
.intro_title {
  margin: 10px 50px 10px 50px;
  font-size: 70px !important;
  line-height: 100px;
  text-align: center;
}
</style>
