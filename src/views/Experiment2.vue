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
        style="object-fit:cover; height: 85vh"
      />
    </v-card-text>

    <v-card-actions>
      <v-btn
        v-if="!isPlaying && !isComplete"
        style="position: fixed; top: 10px;"
        fab
        @click="initAudio()"
      >
        ▶︎
      </v-btn>
      <v-btn
        v-if="isComplete"
        outlined
        rounded
        style="position: fixed; bottom: 10px; width: 20%; font-size: 30px; padding: 30px;"
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
      null,
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
      null,
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
    ], [
      null,
      require('@/assets/나이테/3/001.png'),
      require('@/assets/나이테/3/002.png'),
      require('@/assets/나이테/3/003.png'),
      require('@/assets/나이테/3/004.png'),
      require('@/assets/나이테/3/005.png'),
      require('@/assets/나이테/3/006.png'),
      require('@/assets/나이테/3/007.png'),
      require('@/assets/나이테/3/008.png'),
      require('@/assets/나이테/3/009.png'),
      require('@/assets/나이테/3/010.png'),
      require('@/assets/나이테/3/011.png'),
      require('@/assets/나이테/3/012.png'),
      require('@/assets/나이테/3/013.png'),
    ], [
      null,
      require('@/assets/나이테/4/001.png'),
      require('@/assets/나이테/4/002.png'),
      require('@/assets/나이테/4/003.png'),
      require('@/assets/나이테/4/004.png'),
      require('@/assets/나이테/4/005.png'),
      require('@/assets/나이테/4/006.png'),
      require('@/assets/나이테/4/007.png'),
      require('@/assets/나이테/4/008.png'),
      require('@/assets/나이테/4/009.png'),
      require('@/assets/나이테/4/010.png'),
      require('@/assets/나이테/4/011.png'),
      require('@/assets/나이테/4/012.png'),
      require('@/assets/나이테/4/013.png'),
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
        this.playAudio('나이테-유아선생님');
      } else if (this.condition === '3') {
        this.playAudio('나이테-선생님 with Peer');
      } else if (this.condition === '4') {
        this.playAudio('나이테-유아선생님 with Peer');
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
      const imgs = this.imgs[this.condition - 1];
      if (this.isPlaying) {
        setTimeout(() => {
          let img;
          if (this.condition === '1') {
            if (currentTime > 0 && currentTime < 4) {
              img = 9;
            } else if (currentTime > 4 && currentTime < 16) {
              img = 1;
            } else if (currentTime > 16 && currentTime < 24) {
              img = 2;
            } else if (currentTime > 24 && currentTime < 36) {
              img = 3;
            } else if (currentTime > 36 && currentTime < 50) {
              img = 4;
            } else if (currentTime > 50 && currentTime < 58) {
              img = 5;
            } else if (currentTime > 58 && currentTime < 62) {
              img = 6;
            } else if (currentTime > 62 && currentTime < 70) {
              img = 7;
            } else if (currentTime > 70 && currentTime < 79) {
              img = 8;
            } else if (currentTime > 79 && currentTime < 82) {
              img = 10;
            } else if (currentTime > 82) {
              img = 10;
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 5) {
              img = 9;
            } else if (currentTime > 5 && currentTime < 16) {
              img = 1;
            } else if (currentTime > 16 && currentTime < 25) {
              img = 2;
            } else if (currentTime > 25 && currentTime < 38) {
              img = 3;
            } else if (currentTime > 38 && currentTime < 47) {
              img = 4;
            } else if (currentTime > 47 && currentTime < 55) {
              img = 5;
            } else if (currentTime > 55 && currentTime < 61) {
              img = 6;
            } else if (currentTime > 61 && currentTime < 67) {
              img = 7;
            } else if (currentTime > 67 && currentTime < 77) {
              img = 8;
            } else if (currentTime > 77 && currentTime < 82) {
              img = 10;
            } else if (currentTime > 82) {
              img = 10;
              this.isComplete = true;
            }
          } else if (this.condition === '3') {
            if (currentTime > 0 && currentTime < 5) {
              img = 12;
            } else if (currentTime > 5 && currentTime < 13) {
              img = 1;
            } else if (currentTime > 13 && currentTime < 20) {
              img = 2;
            } else if (currentTime > 20 && currentTime < 24) {
              img = 1;
            } else if (currentTime > 24 && currentTime < 33) {
              img = 3;
            } else if (currentTime > 33 && currentTime < 46) {
              img = 4;
            } else if (currentTime > 46 && currentTime < 55) {
              img = 5;
            } else if (currentTime > 55 && currentTime < 66) {
              img = 6;
            } else if (currentTime > 66 && currentTime < 75) {
              img = 7;
            } else if (currentTime > 75 && currentTime < 81) {
              img = 8;
            } else if (currentTime > 81 && currentTime < 87) {
              img = 9;
            } else if (currentTime > 87 && currentTime < 92) {
              img = 10;
            } else if (currentTime > 92 && currentTime < 103) {
              img = 11;
            } else if (currentTime > 103 && currentTime < 108) {
              img = 13;
            } else if (currentTime > 108) {
              img = 13;
              this.isComplete = true;
            }
          } else if (this.condition === '4') {
            if (currentTime > 0 && currentTime < 5) {
              img = 12;
            } else if (currentTime > 5 && currentTime < 12) {
              img = 1;
            } else if (currentTime > 12 && currentTime < 20) {
              img = 2;
            } else if (currentTime > 20 && currentTime < 24) {
              img = 1;
            } else if (currentTime > 24 && currentTime < 33) {
              img = 3;
            } else if (currentTime > 33 && currentTime < 46) {
              img = 4;
            } else if (currentTime > 46 && currentTime < 55) {
              img = 5;
            } else if (currentTime > 55 && currentTime < 66) {
              img = 6;
            } else if (currentTime > 66 && currentTime < 75) {
              img = 7;
            } else if (currentTime > 75 && currentTime < 81) {
              img = 8;
            } else if (currentTime > 81 && currentTime < 88) {
              img = 9;
            } else if (currentTime > 88 && currentTime < 93) {
              img = 10;
            } else if (currentTime > 93 && currentTime < 102) {
              img = 11;
            } else if (currentTime > 102 && currentTime < 108) {
              img = 13;
            } else if (currentTime > 108) {
              img = 13;
              this.isComplete = true;
            }
          }
          this.img = imgs[img];
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
