<template>
  <v-card
    class="mx-auto"
    max-width="750"
    outlined
  >
    <v-card-text>
      <div class="px-8 py-5 body-1">
        <div class="text-center">
          마지막으로 아래 링크로 들어가셔서 설문을 완료해 주셔야 실험이 종료됩니다.
        </div>
        <div class="text-center">
          ‘설문하러 가기’ 버튼을 누르시면 설문 페이지로 연결됩니다.
        </div>
        <div class="text-center">
          <b>[설문까지 완료하셔야만 실험비 지급이 가능해서 꼭 설문 완료 부탁드립니다!!.]</b>
        </div>
      </div>
    </v-card-text>

    <v-card-actions>
      <v-btn
        block
        color="primary"
        :loading="isSending"
        @click="goSurvey"
      >
        설문하러 가기 {{ countDown > 0 ? `(${countDown})` : '' }}
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
/* eslint-disable import/no-unresolved */

export default {
  data: () => ({
    isSubmitted: false,
    isSending: false,
    countDown: 5,
    filesystem: null,
    stylizedObjects: {},
  }),

  computed: {
    condition() {
      return this.$store.state.data.experimentType;
    },
  },

  async mounted() {
    this.submitData();
    this.countDownTimer();
    setTimeout(() => {
      // this.goSurvey();
      // if (!this.isSending) {
      //   const url = 'https://docs.google.com/forms/d/e/1FAIpQLScJc0mV0mqJBh-TYiXZb4jDbsMf-exUKO3yJieu-zunOv3tUQ/viewform';
      //   window.location = url;
      // }
    }, this.countDown * 1000);
  },

  methods: {
    async submitData() {
      this.isSending = true;
      const data = this.$store.getters.getSubmitData;
      console.log(data, this.$store);
      try {
        await this.$firebase.database().ref(`response/${this.$store.state.data.participantID}`).set(data);
      } catch (err) {
        console.log(err);
      } finally {
        this.isSending = false;
      }
    },

    goSurvey() {
      let url = '';
      if (this.condition === '1') {
        url = 'https://forms.gle/';
      } else if (this.condition === '2') {
        url = 'https://forms.gle/';
      } else if (this.condition === '3') {
        url = 'https://forms.gle/';
      } else if (this.condition === '4') {
        url = 'https://forms.gle/';
      }
      window.open(url);
    },

    countDownTimer() {
      if (this.countDown > 0) {
        setTimeout(() => {
          this.countDown -= 1;
          this.countDownTimer();
        }, 1000);
      }
    },
  },
};
</script>
