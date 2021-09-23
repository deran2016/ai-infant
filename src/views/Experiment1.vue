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
      null,
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
      null,
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
    ], [
      null,
      require('@/assets/얼음/3/001.png'),
      require('@/assets/얼음/3/002.png'),
      require('@/assets/얼음/3/003.png'),
      require('@/assets/얼음/3/004.png'),
      require('@/assets/얼음/3/005.png'),
      require('@/assets/얼음/3/006.png'),
      require('@/assets/얼음/3/007.png'),
      require('@/assets/얼음/3/008.png'),
      require('@/assets/얼음/3/009.png'),
      require('@/assets/얼음/3/010.png'),
      require('@/assets/얼음/3/011.png'),
      require('@/assets/얼음/3/012.png'),
      require('@/assets/얼음/3/013.png'),
      require('@/assets/얼음/3/014.png'),
      require('@/assets/얼음/3/015.png'),
      require('@/assets/얼음/3/016.png'),
    ], [
      null,
      require('@/assets/얼음/4/001.png'),
      require('@/assets/얼음/4/002.png'),
      require('@/assets/얼음/4/003.png'),
      require('@/assets/얼음/4/004.png'),
      require('@/assets/얼음/4/005.png'),
      require('@/assets/얼음/4/006.png'),
      require('@/assets/얼음/4/007.png'),
      require('@/assets/얼음/4/008.png'),
      require('@/assets/얼음/4/009.png'),
      require('@/assets/얼음/4/010.png'),
      require('@/assets/얼음/4/011.png'),
      require('@/assets/얼음/4/012.png'),
      require('@/assets/얼음/4/013.png'),
      require('@/assets/얼음/4/014.png'),
      require('@/assets/얼음/4/015.png'),
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
        this.img = require('@/assets/얼음/3/009.png');
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
      const imgs = this.imgs[this.condition - 1];
      if (this.isPlaying) {
        setTimeout(() => {
          let img;
          if (this.condition === '1') {
            if (currentTime > 0 && currentTime < 6) {
              img = 7;
            } else if (currentTime > 6 && currentTime < 12) {
              img = 8;
            } else if (currentTime > 12 && currentTime < 19) {
              img = 1;
            } else if (currentTime > 19 && currentTime < 28) {
              img = 2;
            } else if (currentTime > 28 && currentTime < 35) {
              img = 3;
            } else if (currentTime > 35 && currentTime < 39) {
              img = 1;
            } else if (currentTime > 39 && currentTime < 50) {
              img = 4;
            } else if (currentTime > 50 && currentTime < 56) {
              img = 5;
            } else if (currentTime > 56 && currentTime < 68) {
              img = 4;
            } else if (currentTime > 68 && currentTime < 72) {
              img = 6;
            } else if (currentTime > 72 && currentTime < 77) {
              img = 9;
            } else if (currentTime > 77 && currentTime < 80) {
              img = 10;
            } else if (currentTime > 80) {
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 5) {
              img = 7;
            } else if (currentTime > 5 && currentTime < 11) {
              img = 8;
            } else if (currentTime > 11 && currentTime < 18) {
              img = 1;
            } else if (currentTime > 18 && currentTime < 25) {
              img = 2;
            } else if (currentTime > 25 && currentTime < 31) {
              img = 3;
            } else if (currentTime > 31 && currentTime < 36) {
              img = 1;
            } else if (currentTime > 36 && currentTime < 46) {
              img = 4;
            } else if (currentTime > 46 && currentTime < 51) {
              img = 5;
            } else if (currentTime > 51 && currentTime < 62) {
              img = 4;
            } else if (currentTime > 62 && currentTime < 63) {
              img = 6;
            } else if (currentTime > 63 && currentTime < 72) {
              img = 9;
            } else if (currentTime > 72 && currentTime < 80) {
              img = 10;
            } else if (currentTime > 80) {
              this.isComplete = true;
            }
          } else if (this.condition === '3') {
            if (currentTime > 0 && currentTime < 6) {
              img = 9;
            } else if (currentTime > 6 && currentTime < 10) {
              img = 10;
            } else if (currentTime > 10 && currentTime < 14) {
              img = 11;
            } else if (currentTime > 14 && currentTime < 18) {
              img = 12;
            } else if (currentTime > 18 && currentTime < 24) {
              img = 13;
            } else if (currentTime > 24 && currentTime < 31) {
              img = 1;
            } else if (currentTime > 31 && currentTime < 37) {
              img = 3;
            } else if (currentTime > 37 && currentTime < 47) {
              img = 4;
            } else if (currentTime > 47 && currentTime < 52) {
              img = 2;
            } else if (currentTime > 52 && currentTime < 61) {
              img = 5;
            } else if (currentTime > 61 && currentTime < 66) {
              img = 6;
            } else if (currentTime > 66 && currentTime < 71) {
              img = 7;
            } else if (currentTime > 71 && currentTime < 82) {
              img = 5;
            } else if (currentTime > 82 && currentTime < 86) {
              img = 8;
            } else if (currentTime > 86 && currentTime < 91) {
              img = 14;
            } else if (currentTime > 91 && currentTime < 97) {
              img = 15;
            } else if (currentTime > 97 && currentTime < 106) {
              img = 16;
            } else if (currentTime > 106) {
              this.isComplete = true;
            }
          } else if (this.condition === '4') {
            if (currentTime > 0 && currentTime < 7) {
              img = 8;
            } else if (currentTime > 7 && currentTime < 10) {
              img = 9;
            } else if (currentTime > 10 && currentTime < 14) {
              img = 10;
            } else if (currentTime > 14 && currentTime < 17) {
              img = 11;
            } else if (currentTime > 17 && currentTime < 23) {
              img = 12;
            } else if (currentTime > 23 && currentTime < 31) {
              img = 1;
            } else if (currentTime > 31 && currentTime < 39) {
              img = 2;
            } else if (currentTime > 39 && currentTime < 49) {
              img = 3;
            } else if (currentTime > 49 && currentTime < 54) {
              img = 5;
            } else if (currentTime > 54 && currentTime < 63) {
              img = 4;
            } else if (currentTime > 63 && currentTime < 66) {
              img = 5;
            } else if (currentTime > 66 && currentTime < 74) {
              img = 6;
            } else if (currentTime > 74 && currentTime < 85) {
              img = 4;
            } else if (currentTime > 85 && currentTime < 90) {
              img = 7;
            } else if (currentTime > 90 && currentTime < 95) {
              img = 13;
            } else if (currentTime > 95 && currentTime < 101) {
              img = 14;
            } else if (currentTime > 101 && currentTime < 109) {
              img = 15;
            } else if (currentTime > 109) {
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
