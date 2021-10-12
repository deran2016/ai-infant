<template>
  <v-card
    class="mx-auto"
    max-width="750"
    outlined
  >
    <v-card-text>
      <div class="title text-center">
        실험 참여 안내
      </div>
      <div class="px-8 py-5 body-1">
        <span
          v-for="(item, key) in text[page]"
          :key="key"
        >
          {{ item }} <br />
        </span>
      </div>
    </v-card-text>

    <v-card-actions>
      <v-btn
        block
        color="primary"
        :disabled="disabled"
        @click="next"
      >
        다음 {{ countDown > 0 ? `(${countDown})` : '' }}
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
/* eslint-disable import/no-unresolved */
import { setTimeout } from 'timers';

export default {
  data: () => ({
    countDown: 7,
    page: 0,
    text: [
      [
        '이 앱은 사진과 음성으로 과학 이야기를 배우게 됩니다.',
        '따라서 핸드폰 화면이 꺼지지 않도록 유의해주시고,',
        '소리가 잘 들릴 수 있도록 볼륨을 설정해주세요.',
        '',
        '한글을 못읽는 어린이는 부모님께서 도와주시기 바랍니다',
        '또한 아이가 집중할 수 있는 조용한 공간이면서, 편안하게',
        ' 활동할 수 있도록 식탁이나 책상등에서 실시해주세요.',
      ],
      [
        '아이가 단어를 모르거나 잘 못들었다면 부모님께서 도와주세요.',
        '하지만 과학 문제를 틀리거나 잘 몰라도 개입하지 말아주시고',
        '과학 내용에 대해서는 아는대로 스스로 답변할 수 있도록 해주세요.',
        '',
        '모든 활동이 끝나면 설문을 합니다.',
        '설문은 아이가 부모님의 도움을 받으며 하는 부분과',
        '부모님께서 혼자 하시는 부분으로 나뉘어집니다.',
        '',
        '설문의 결과는 실험 참여와 관련되어 아무런 영향을',
        '미치지 않으니 솔직한 답변 부탁드립니다.',
        '그럼 지금부터 실험을 시작하겠습니다.',
      ],
    ],
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
      this.$router.push({ name: 'Intro' });
    },

    next() {
      if (this.page === 0) {
        this.page += 1;
      } else {
        this.submit();
      }
    },

    countDownTimer() {
      if (this.$router.history.current.name === 'Explanation') {
        if (this.countDown > 0) {
          setTimeout(() => {
            this.countDown -= 1;
            this.countDownTimer();
          }, 1000);
        }
      }
    },

    resetTimer() {
      this.countDown = 7;
    },
  },
};
</script>
