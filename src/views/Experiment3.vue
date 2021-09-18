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
        @click="initAudio();"
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
        this.playAudio('잎 모양-선생님');
      } else if (this.condition === '3') {
        this.playAudio('잎 모양-선생님');
      } else if (this.condition === '4') {
        this.playAudio('잎 모양-선생님');
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
            if (currentTime > 0 && currentTime < 5) {
              this.img = require('@/assets/잎/1/001.png');
            } else if (currentTime > 5 && currentTime < 11) {
              this.img = require('@/assets/잎/1/002.png');
            } else if (currentTime > 11 && currentTime < 20) {
              this.img = require('@/assets/잎/1/003.png');
            } else if (currentTime > 20 && currentTime < 30) {
              this.img = require('@/assets/잎/1/004.png');
            } else if (currentTime > 30 && currentTime < 38) {
              this.img = require('@/assets/잎/1/001.png');
            } else if (currentTime > 38 && currentTime < 45) {
              this.img = require('@/assets/잎/1/005.png');
            } else if (currentTime > 45 && currentTime < 47) {
              this.img = require('@/assets/잎/1/006.png');
            } else if (currentTime > 47 && currentTime < 54) {
              this.img = require('@/assets/잎/1/007.png');
            } else if (currentTime > 54 && currentTime < 64) {
              this.img = require('@/assets/잎/1/009.png');
            } else if (currentTime > 64 && currentTime < 70) {
              this.img = require('@/assets/잎/1/010.png');
            } else if (currentTime > 70 && currentTime < 79) {
              this.img = require('@/assets/잎/1/011.png');
            } else if (currentTime > 79 && currentTime < 86) {
              this.img = require('@/assets/잎/1/012.png');
            } else if (currentTime > 86 && currentTime < 88) {
              this.img = require('@/assets/잎/1/013.png');
            } else if (currentTime > 88) {
              this.isComplete = true;
            }
          } else if (this.condition === '2') {
            if (currentTime > 0 && currentTime < 5) {
              this.img = require('@/assets/잎/2/001.png');
            } else if (currentTime > 5 && currentTime < 11) {
              this.img = require('@/assets/잎/2/002.png');
            } else if (currentTime > 11 && currentTime < 21) {
              this.img = require('@/assets/잎/2/003.png');
            } else if (currentTime > 21 && currentTime < 30) {
              this.img = require('@/assets/잎/2/004.png');
            } else if (currentTime > 30 && currentTime < 39) {
              this.img = require('@/assets/잎/2/001.png');
            } else if (currentTime > 39 && currentTime < 45) {
              this.img = require('@/assets/잎/2/005.png');
            } else if (currentTime > 45 && currentTime < 48) {
              this.img = require('@/assets/잎/2/006.png');
            } else if (currentTime > 48 && currentTime < 55) {
              this.img = require('@/assets/잎/2/007.png');
            } else if (currentTime > 55 && currentTime < 65) {
              this.img = require('@/assets/잎/2/009.png');
            } else if (currentTime > 65 && currentTime < 70) {
              this.img = require('@/assets/잎/2/010.png');
            } else if (currentTime > 70 && currentTime < 78) {
              this.img = require('@/assets/잎/2/011.png');
            } else if (currentTime > 78 && currentTime < 86) {
              this.img = require('@/assets/잎/2/012.png');
            } else if (currentTime > 86 && currentTime < 88) {
              this.img = require('@/assets/잎/2/013.png');
            } else if (currentTime > 88) {
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
