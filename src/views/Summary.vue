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
        style="object-fit:cover; height: 80vh"
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
      require('@/assets/요약/선생님/001.png'),
      require('@/assets/요약/선생님/002.png'),
      require('@/assets/요약/선생님/003.png'),
      require('@/assets/요약/선생님/004.png'),
      require('@/assets/요약/선생님/005.png'),
      require('@/assets/요약/선생님/006.png'),
      require('@/assets/요약/선생님/007.png'),
      require('@/assets/요약/선생님/008.png'),
      require('@/assets/요약/선생님/009.png'),
      require('@/assets/요약/선생님/010.png'),
      require('@/assets/요약/선생님/011.png'),
      require('@/assets/요약/선생님/012.png'),
    ], [
      null,
      require('@/assets/요약/유아선생님/001.png'),
      require('@/assets/요약/유아선생님/002.png'),
      require('@/assets/요약/유아선생님/003.png'),
      require('@/assets/요약/유아선생님/004.png'),
      require('@/assets/요약/유아선생님/005.png'),
      require('@/assets/요약/유아선생님/006.png'),
      require('@/assets/요약/유아선생님/007.png'),
      require('@/assets/요약/유아선생님/008.png'),
      require('@/assets/요약/유아선생님/009.png'),
      require('@/assets/요약/유아선생님/010.png'),
      require('@/assets/요약/유아선생님/011.png'),
      require('@/assets/요약/유아선생님/012.png'),
    ], [
      null,
      require('@/assets/요약/유아/001.png'),
      require('@/assets/요약/유아/002.png'),
      require('@/assets/요약/유아/003.png'),
      require('@/assets/요약/유아/004.png'),
      require('@/assets/요약/유아/005.png'),
      require('@/assets/요약/유아/006.png'),
      require('@/assets/요약/유아/007.png'),
      require('@/assets/요약/유아/008.png'),
      require('@/assets/요약/유아/009.png'),
      require('@/assets/요약/유아/010.png'),
      require('@/assets/요약/유아/011.png'),
      require('@/assets/요약/유아/012.png'),
      require('@/assets/요약/유아/013.png'),
      require('@/assets/요약/유아/014.png'),
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
      this.$router.push({ name: 'Submit' });
    },

    initImg() {
      if (this.condition === '1') {
        this.img = require('@/assets/요약/선생님/001.png');
      } else if (this.condition === '2') {
        this.img = require('@/assets/요약/유아선생님/001.png');
      } else if (this.condition === '3' || this.condition === '4') {
        this.img = require('@/assets/요약/유아/001.png');
      }
    },

    initAudio() {
      if (this.condition === '1') {
        this.playAudio('요약-선생님');
      } else if (this.condition === '2') {
        this.playAudio('요약-유아선생님');
      } else if (this.condition === '3') {
        this.playAudio('요약-유아');
      } else if (this.condition === '4') {
        this.playAudio('요약-유아2');
      }
    },

    preloadImg() {
      if (this.condition === '4') {
        for (let i = 0; i < this.imgs[2].length; i += 1) {
          const img = new Image();
          img.src = this.imgs[2][i];
        }
      } else {
        for (let i = 0; i < this.imgs[this.condition - 1].length; i += 1) {
          const img = new Image();
          img.src = this.imgs[this.condition - 1][i];
        }
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
      const imgs = this.condition === '4' ? this.imgs[2] : this.imgs[this.condition - 1];
      if (this.isPlaying) {
        setTimeout(() => {
          let img;
          if (this.condition === '1') {
            if (currentTime > 0 && currentTime < 5) {
              img = 1;
            } else if (currentTime > 5 && currentTime < 13) {
              img = 2;
            } else if (currentTime > 13 && currentTime < 18) {
              img = 3;
            } else if (currentTime > 18 && currentTime < 22) {
              img = 4;
            } else if (currentTime > 22 && currentTime < 29) {
              img = 5;
            } else if (currentTime > 29 && currentTime < 36) {
              img = 6;
            } else if (currentTime > 36 && currentTime < 41) {
              img = 7;
            } else if (currentTime > 41 && currentTime < 47) {
              img = 8;
            } else if (currentTime > 47 && currentTime < 53) {
              img = 9;
            } else if (currentTime > 53 && currentTime < 64) {
              img = 10;
            } else if (currentTime > 64 && currentTime < 68) {
              img = 11;
            } else if (currentTime > 68 && currentTime < 77) {
              img = 12;
            } else if (currentTime > 77) {
              img = 12;
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 5) {
              img = 1;
            } else if (currentTime > 5 && currentTime < 13) {
              img = 2;
            } else if (currentTime > 13 && currentTime < 18) {
              img = 3;
            } else if (currentTime > 18 && currentTime < 22) {
              img = 4;
            } else if (currentTime > 22 && currentTime < 30) {
              img = 5;
            } else if (currentTime > 30 && currentTime < 37) {
              img = 6;
            } else if (currentTime > 37 && currentTime < 44) {
              img = 7;
            } else if (currentTime > 44 && currentTime < 50) {
              img = 8;
            } else if (currentTime > 50 && currentTime < 57) {
              img = 9;
            } else if (currentTime > 57 && currentTime < 67) {
              img = 10;
            } else if (currentTime > 67 && currentTime < 72) {
              img = 11;
            } else if (currentTime > 72 && currentTime < 81) {
              img = 12;
            } else if (currentTime > 81) {
              img = 12;
              this.isComplete = true;
            }
          } else if (this.condition === '3' || this.condition === '4') {
            if (currentTime > 0 && currentTime < 7) {
              img = 1;
            } else if (currentTime > 7 && currentTime < 13) {
              img = 2;
            } else if (currentTime > 13 && currentTime < 19) {
              img = 3;
            } else if (currentTime > 19 && currentTime < 25) {
              img = 4;
            } else if (currentTime > 25 && currentTime < 29) {
              img = 5;
            } else if (currentTime > 29 && currentTime < 37) {
              img = 6;
            } else if (currentTime > 37 && currentTime < 46) {
              img = 7;
            } else if (currentTime > 46 && currentTime < 52) {
              img = 8;
            } else if (currentTime > 52 && currentTime < 57) {
              img = 9;
            } else if (currentTime > 57 && currentTime < 64) {
              img = 10;
            } else if (currentTime > 64 && currentTime < 74) {
              img = 11;
            } else if (currentTime > 74 && currentTime < 78) {
              img = 12;
            } else if (currentTime > 78 && currentTime < 83) {
              img = 13;
            } else if (currentTime > 83 && currentTime < 92) {
              img = 14;
            } else if (currentTime > 82) {
              img = 14;
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
body * { touch-action: auto; }
</style>
