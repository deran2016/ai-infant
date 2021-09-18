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
    img: null,
    imgs: [[
      require('@/assets/얼음/1/001.png'),
      require('@/assets/얼음/1/002.png'),
      require('@/assets/얼음/1/003.png'),
      require('@/assets/얼음/1/004.png'),
      require('@/assets/얼음/1/005.png'),
      require('@/assets/얼음/1/006.png'),
      require('@/assets/얼음/1/007.png'),
      require('@/assets/얼음/1/008.png'),
      require('@/assets/얼음/1/009.png'),
      require('@/assets/얼음/1/010.png'),
    ], [
      require('@/assets/얼음/2/001.png'),
      require('@/assets/얼음/2/002.png'),
      require('@/assets/얼음/2/003.png'),
      require('@/assets/얼음/2/004.png'),
      require('@/assets/얼음/2/005.png'),
      require('@/assets/얼음/2/006.png'),
      require('@/assets/얼음/2/007.png'),
      require('@/assets/얼음/2/008.png'),
      require('@/assets/얼음/2/009.png'),
      require('@/assets/얼음/2/010.png'),
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
      this.$router.push({ name: 'Experiment2' });
    },

    initImg() {
      if (this.condition === '1') {
        this.img = require('@/assets/얼음/1/007.png');
      } else if (this.condition === '2') {
        this.img = require('@/assets/얼음/2/007.png');
      } else if (this.condition === '3') {
        this.img = require('@/assets/얼음/1/007.png');
      } else if (this.condition === '4') {
        this.img = require('@/assets/얼음/2/007.png');
      }
    },

    initAudio() {
      if (this.condition === '1') {
        this.playAudio('얼음-선생님');
      } else if (this.condition === '2') {
        this.playAudio('얼음-유아선생님');
      } else if (this.condition === '3') {
        this.playAudio('얼음-선생님 with Peer');
      } else if (this.condition === '4') {
        this.playAudio('얼음-유아선생님 with Peer');
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
            if (currentTime > 0 && currentTime < 6) {
              this.img = require('@/assets/얼음/1/007.png');
            } else if (currentTime > 6 && currentTime < 12) {
              this.img = require('@/assets/얼음/1/008.png');
            } else if (currentTime > 12 && currentTime < 19) {
              this.img = require('@/assets/얼음/1/001.png');
            } else if (currentTime > 19 && currentTime < 28) {
              this.img = require('@/assets/얼음/1/002.png');
            } else if (currentTime > 28 && currentTime < 35) {
              this.img = require('@/assets/얼음/1/003.png');
            } else if (currentTime > 35 && currentTime < 39) {
              this.img = require('@/assets/얼음/1/001.png');
            } else if (currentTime > 39 && currentTime < 50) {
              this.img = require('@/assets/얼음/1/004.png');
            } else if (currentTime > 50 && currentTime < 56) {
              this.img = require('@/assets/얼음/1/005.png');
            } else if (currentTime > 56 && currentTime < 68) {
              this.img = require('@/assets/얼음/1/004.png');
            } else if (currentTime > 68 && currentTime < 72) {
              this.img = require('@/assets/얼음/1/006.png');
            } else if (currentTime > 72 && currentTime < 77) {
              this.img = require('@/assets/얼음/1/009.png');
            } else if (currentTime > 77 && currentTime < 80) {
              this.img = require('@/assets/얼음/1/010.png');
            } else if (currentTime > 80) {
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 5) {
              this.img = require('@/assets/얼음/2/007.png');
            } else if (currentTime > 5 && currentTime < 11) {
              this.img = require('@/assets/얼음/2/008.png');
            } else if (currentTime > 11 && currentTime < 18) {
              this.img = require('@/assets/얼음/2/001.png');
            } else if (currentTime > 18 && currentTime < 25) {
              this.img = require('@/assets/얼음/2/002.png');
            } else if (currentTime > 25 && currentTime < 31) {
              this.img = require('@/assets/얼음/2/003.png');
            } else if (currentTime > 31 && currentTime < 36) {
              this.img = require('@/assets/얼음/2/001.png');
            } else if (currentTime > 36 && currentTime < 46) {
              this.img = require('@/assets/얼음/2/004.png');
            } else if (currentTime > 46 && currentTime < 51) {
              this.img = require('@/assets/얼음/2/005.png');
            } else if (currentTime > 51 && currentTime < 62) {
              this.img = require('@/assets/얼음/2/004.png');
            } else if (currentTime > 62 && currentTime < 63) {
              this.img = require('@/assets/얼음/2/006.png');
            } else if (currentTime > 63 && currentTime < 72) {
              this.img = require('@/assets/얼음/2/009.png');
            } else if (currentTime > 72 && currentTime < 80) {
              this.img = require('@/assets/얼음/2/010.png');
            } else if (currentTime > 80) {
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
