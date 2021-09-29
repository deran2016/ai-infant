<template>
  <v-card
    class="mx-auto"
    max-width="750"
    outlined
  >
    <v-card-text>
      <v-row
        rows="12"
        sm="6"
      >
        <v-col>
          <h1 class="intro_title">
            재미있는
            <br />
            과학 이야기
          </h1>
          <v-btn
            outlined
            rounded
            style="width: 100%; font-size: 30px; padding: 30px; margin: 0 auto;"
            color="primary"
            @click="submit"
          >
            학습 시작 ▶︎
          </v-btn>
        </v-col>
        <div class="img-container">
          <img
            class="intro_img"
            :src="selectImage()"
            style="width: 100%; height: auto;"
          />
        </div>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
/* eslint-disable import/no-extraneous-dependencies */
/* eslint-disable import/no-unresolved */
/* eslint-disable global-require */
import { setTimeout } from 'timers';

export default {
  data: () => ({
    countDown: 7,
  }),

  computed: {
    disabled() {
      return this.countDown > 0;
    },
    condition() {
      return this.$store.state.data.experimentType;
    },
  },

  mounted() {
    this.countDownTimer();
  },

  methods: {
    submit() {
      this.$router.push({ name: 'Experiment1' });
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
      if (this.condition === '1' || this.condition === '3') {
        image = require('@/assets/img/3.png');
      } else if (this.condition === '2' || this.condition === '4') {
        image = require('@/assets/img/유아선생님.jpg');
      }
      return image;
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
  margin: 10px 10px 10px 10px;
  font-size: 50px !important;
  line-height: 83px;
  text-align: center;
}
.img-container {
  width: 45%;
  overflow: hidden;
}
</style>
