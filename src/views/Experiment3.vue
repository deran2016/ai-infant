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
        @click="initAudio();"
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
    img: null,
    imgs: [[
      null,
      require('@/assets/잎/1/001.png'),
      require('@/assets/잎/1/002.png'),
      require('@/assets/잎/1/003.png'),
      require('@/assets/잎/1/004.png'),
      require('@/assets/잎/1/005.png'),
      require('@/assets/잎/1/006.png'),
      require('@/assets/잎/1/007.png'),
      require('@/assets/잎/1/008.png'),
      require('@/assets/잎/1/009.png'),
      require('@/assets/잎/1/010.png'),
      require('@/assets/잎/1/011.png'),
      require('@/assets/잎/1/012.png'),
      require('@/assets/잎/1/013.png'),
    ], [
      null,
      require('@/assets/잎/2/001.png'),
      require('@/assets/잎/2/002.png'),
      require('@/assets/잎/2/003.png'),
      require('@/assets/잎/2/004.png'),
      require('@/assets/잎/2/005.png'),
      require('@/assets/잎/2/006.png'),
      require('@/assets/잎/2/007.png'),
      require('@/assets/잎/2/008.png'),
      require('@/assets/잎/2/009.png'),
      require('@/assets/잎/2/010.png'),
      require('@/assets/잎/2/011.png'),
      require('@/assets/잎/2/012.png'),
      require('@/assets/잎/2/013.png'),
    ], [
      null,
      require('@/assets/잎/3/001.png'),
      require('@/assets/잎/3/002.png'),
      require('@/assets/잎/3/003.png'),
      require('@/assets/잎/3/004.png'),
      require('@/assets/잎/3/005.png'),
      require('@/assets/잎/3/006.png'),
      require('@/assets/잎/3/007.png'),
      require('@/assets/잎/3/008.png'),
      require('@/assets/잎/3/009.png'),
      require('@/assets/잎/3/010.png'),
      require('@/assets/잎/3/011.png'),
      require('@/assets/잎/3/012.png'),
      require('@/assets/잎/3/013.png'),
      require('@/assets/잎/3/014.png'),
      require('@/assets/잎/3/015.png'),
      require('@/assets/잎/3/016.png'),
      require('@/assets/잎/3/017.png'),
    ], [
      null,
      require('@/assets/잎/4/001.png'),
      require('@/assets/잎/4/002.png'),
      require('@/assets/잎/4/003.png'),
      require('@/assets/잎/4/004.png'),
      require('@/assets/잎/4/005.png'),
      require('@/assets/잎/4/006.png'),
      require('@/assets/잎/4/007.png'),
      require('@/assets/잎/4/008.png'),
      require('@/assets/잎/4/009.png'),
      require('@/assets/잎/4/010.png'),
      require('@/assets/잎/4/011.png'),
      require('@/assets/잎/4/012.png'),
      require('@/assets/잎/4/013.png'),
      require('@/assets/잎/4/014.png'),
      require('@/assets/잎/4/015.png'),
      require('@/assets/잎/4/016.png'),
      require('@/assets/잎/4/017.png'),
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
      this.$router.push({ name: 'Quiz' });
    },

    initImg() {
      if (this.condition === '1') {
        this.img = require('@/assets/잎/1/001.png');
      } else if (this.condition === '2') {
        this.img = require('@/assets/잎/2/001.png');
      } else if (this.condition === '3') {
        this.img = require('@/assets/잎/1/001.png');
      } else if (this.condition === '4') {
        this.img = require('@/assets/잎/2/001.png');
      }
    },

    initAudio() {
      if (this.condition === '1') {
        this.playAudio('잎 모양-선생님');
      } else if (this.condition === '2') {
        this.playAudio('잎 모양-유아선생님');
      } else if (this.condition === '3') {
        this.playAudio('잎 모양-선생님 with Peer');
      } else if (this.condition === '4') {
        this.playAudio('잎 모양-유아선생님 with Peer');
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
            if (currentTime > 0 && currentTime < 5) {
              img = 1;
            } else if (currentTime > 5 && currentTime < 11) {
              img = 2;
            } else if (currentTime > 11 && currentTime < 20) {
              img = 3;
            } else if (currentTime > 20 && currentTime < 30) {
              img = 4;
            } else if (currentTime > 30 && currentTime < 38) {
              img = 1;
            } else if (currentTime > 38 && currentTime < 45) {
              img = 5;
            } else if (currentTime > 45 && currentTime < 50) {
              img = 6;
            } else if (currentTime > 50 && currentTime < 54) {
              img = 7;
            } else if (currentTime > 54 && currentTime < 64) {
              img = 9;
            } else if (currentTime > 64 && currentTime < 70) {
              img = 10;
            } else if (currentTime > 70 && currentTime < 79) {
              img = 11;
            } else if (currentTime > 79 && currentTime < 86) {
              img = 12;
            } else if (currentTime > 86 && currentTime < 88) {
              img = 13;
            } else if (currentTime > 88) {
              img = 13;
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 4) {
              img = 1;
            } else if (currentTime > 4 && currentTime < 11) {
              img = 2;
            } else if (currentTime > 11 && currentTime < 20) {
              img = 3;
            } else if (currentTime > 20 && currentTime < 30) {
              img = 4;
            } else if (currentTime > 30 && currentTime < 39) {
              img = 1;
            } else if (currentTime > 39 && currentTime < 46) {
              img = 5;
            } else if (currentTime > 46 && currentTime < 49) {
              img = 6;
            } else if (currentTime > 49 && currentTime < 55) {
              img = 7;
            } else if (currentTime > 55 && currentTime < 65) {
              img = 9;
            } else if (currentTime > 65 && currentTime < 70) {
              img = 10;
            } else if (currentTime > 70 && currentTime < 79) {
              img = 11;
            } else if (currentTime > 79 && currentTime < 86) {
              img = 12;
            } else if (currentTime > 86 && currentTime < 88) {
              img = 13;
            } else if (currentTime > 88) {
              img = 13;
              this.isComplete = true;
            }
          } else if (this.condition === '3') {
            if (currentTime > 0 && currentTime < 5) {
              img = 1;
            } else if (currentTime > 5 && currentTime < 10) {
              img = 2;
            } else if (currentTime > 10 && currentTime < 13) {
              img = 3;
            } else if (currentTime > 13 && currentTime < 17) {
              img = 4;
            } else if (currentTime > 17 && currentTime < 24) {
              img = 3;
            } else if (currentTime > 24 && currentTime < 27) {
              img = 4;
            } else if (currentTime > 27 && currentTime < 34) {
              img = 5;
            } else if (currentTime > 34 && currentTime < 44) {
              img = 6;
            } else if (currentTime > 44 && currentTime < 49) {
              img = 5;
            } else if (currentTime > 49 && currentTime < 54) {
              img = 7;
            } else if (currentTime > 54 && currentTime < 61) {
              img = 1;
            } else if (currentTime > 61 && currentTime < 65) {
              img = 8;
            } else if (currentTime > 65 && currentTime < 68) {
              img = 9;
            } else if (currentTime > 68 && currentTime < 75) {
              img = 10;
            } else if (currentTime > 75 && currentTime < 83) {
              img = 11;
            } else if (currentTime > 83 && currentTime < 88) {
              img = 12;
            } else if (currentTime > 88 && currentTime < 95) {
              img = 13;
            } else if (currentTime > 95 && currentTime < 105) {
              img = 14;
            } else if (currentTime > 105 && currentTime < 117) {
              img = 15;
            } else if (currentTime > 117 && currentTime < 126) {
              img = 16;
            } else if (currentTime > 126 && currentTime < 133) {
              img = 17;
            } else if (currentTime > 133) {
              img = 17;
              this.isComplete = true;
            }
          } else if (this.condition === '4') {
            if (currentTime > 0 && currentTime < 6) {
              img = 1;
            } else if (currentTime > 6 && currentTime < 12) {
              img = 2;
            } else if (currentTime > 12 && currentTime < 15) {
              img = 3;
            } else if (currentTime > 15 && currentTime < 18) {
              img = 4;
            } else if (currentTime > 18 && currentTime < 26) {
              img = 3;
            } else if (currentTime > 26 && currentTime < 29) {
              img = 4;
            } else if (currentTime > 29 && currentTime < 37) {
              img = 5;
            } else if (currentTime > 37 && currentTime < 49) {
              img = 6;
            } else if (currentTime > 49 && currentTime < 58) {
              img = 1;
            } else if (currentTime > 58 && currentTime < 66) {
              img = 7;
            } else if (currentTime > 66 && currentTime < 70) {
              img = 8;
            } else if (currentTime > 70 && currentTime < 72) {
              img = 9;
            } else if (currentTime > 72 && currentTime < 80) {
              img = 10;
            } else if (currentTime > 80 && currentTime < 88) {
              img = 11;
            } else if (currentTime > 88 && currentTime < 94) {
              img = 12;
            } else if (currentTime > 94 && currentTime < 101) {
              img = 13;
            } else if (currentTime > 101 && currentTime < 111) {
              img = 14;
            } else if (currentTime > 111 && currentTime < 123) {
              img = 15;
            } else if (currentTime > 123 && currentTime < 132) {
              img = 16;
            } else if (currentTime > 132 && currentTime < 137) {
              img = 17;
            } else if (currentTime > 137) {
              img = 17;
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
