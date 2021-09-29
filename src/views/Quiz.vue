<template>
  <v-card
    class="mx-auto"
    max-width="95vw"
    outlined
  >
    <v-card-text
      class="row"
    >
      <div
        class="column"
        :class="{circle: correct}"
      >
        <h1 class="quiz_title">
          <span v-if="condition === '1' || condition === '2'">퀴즈! 맞혀보세요!</span>
          <span v-if="condition === '3' || condition === '4'">친구에게 알려주세요!</span>
        </h1>
        <p class="quiz_content">
          <span v-show="!isProblem">
            {{ text }}
          </span>
          <span v-show="isProblem">
            {{ problems[selectProblem][now].problem.split(problems[selectProblem][now].answer)[0] }}
            <span
              v-show="problems[selectProblem][now].problem
                .split(problems[selectProblem][now].answer).length > 1"
              :class="{blind: blind, answer: correct}"
            >
              {{ problems[selectProblem][now].answer }}
            </span>
            {{ problems[selectProblem][now].problem.split(problems[selectProblem][now].answer)[1] }}
          </span>
        </p>
        <div class="row answerlist">
          <v-btn
            v-for="(item, key) in problems[selectProblem][now].list"
            v-show="isProblem"
            :id="item"
            :key="key"
            :class="{check: correct && (problems[selectProblem][now].answer === item),
                     imgbtn: isImageType,
                     hover: true}"
            :disabled="disabled"
            outlined
            rounded
            style="margin-right: 4px; margin-bottom: 10px; font-size: 2vw;"
            @click="checkAnswer(problems[selectProblem][now], item)"
          >
            <span v-show="!isImageType">
              {{ numbering(key + 1) + ' ' + item }}
            </span>
            <img
              v-show="isImageType"
              :src="isImageType ? require(`@/assets/img/${item}`) : ''"
              height="60"
            />
          </v-btn>
        </div>
      </div>
      <img
        class="quiz_img"
        :src="teacherImg"
        height="185"
      />
    </v-card-text>

    <v-card-actions>
      <v-btn
        v-if="!isStarted && !now"
        fab
        @click="initAudio()"
      >
        ▶︎
      </v-btn>
      <v-btn
        v-if="!isStarted && now"
        fab
        @click="playAudio(file, () => {disabled = false})"
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
import { setTimeout } from 'timers';
import { mapMutations } from 'vuex';

export default {
  data: () => ({
    isStarted: false,
    file: '',
    isPlaying: false,
    isProblem: true,
    disabled: true,
    text: '',
    now: 0,
    blind: true,
    correct: false,
    isComplete: false,
    teacherImg: null,
    explain: false,
    countUp: {},
    failCount: {},
    imgs: [
      require('@/assets/img/퀴즈3번1.png'),
      require('@/assets/img/퀴즈3번2.png'),
      require('@/assets/img/퀴즈3번3.png'),
    ],
    problems: [[{
      problem: '손에 물기때문에 얼음이 달라붙는다',
      answer: '물기',
      list: ['물기', '부기', '돌기'],
    }, {
      problem: '우리나라 3살 나무의 나이테는 3개이다.',
      answer: '3개',
      list: ['1개', '3개', '5개'],
    }, {
      problem: '다음중 더운 곳에 사는 식물의 잎은?',
      answer: '퀴즈3번1.png',
      list: ['퀴즈3번1.png', '퀴즈3번2.png', '퀴즈3번3.png'],
    }, {
      problem: '열대지방의 나무들은 나이테가 어떻게 생겼을까?',
      answer: '없다',
      list: ['뾰족한 모양', '네모 모양', '없다'],
    }, {
      problem: '얼음이 손에 달라 붙었다가 왜 금방 떨어질까?',
      answer: '피부가 따뜻해서 얼음이 녹는다.',
      list: ['피부가 차가워서 얼음이 떨어진다.', '피부가 따뜻해서 얼음이 녹는다.', '피부가 부드러워서 얼음이 떨어진다.'],
    }],
    [{
      problem: '왜 붙었는지 너가 알려줄 수 있어?',
      answer: '물기 때문에',
      list: ['물기 때문에', '부기 때문에', '돌기 때문에'],
    }, {
      problem: '우리나라의 3살 짜리 나무는 나이테가 몇 개일까?',
      answer: '3개',
      list: ['1개', '3개', '5개'],
    }, {
      problem: '더운 곳에서 사는 식물을 알려줘!',
      answer: '퀴즈3번1.png',
      list: ['퀴즈3번1.png', '퀴즈3번2.png', '퀴즈3번3.png'],
    }, {
      problem: '날이 덥고 습한 열대지방은 항상 해가 쨍쨍하고 덥대. 나이테는 어떻게 생겼을까?',
      answer: '없다',
      list: ['뾰족한 모양', '네모 모양', '없다'],
    }, {
      problem: '얼음을 손으로 잡으면 손에 달라 붙었다가 왜 금방 떨어질까?',
      answer: '피부가 따뜻해서 얼음이 녹는다.',
      list: ['피부가 차가워서 얼음이 떨어진다.', '피부가 따뜻해서 얼음이 녹는다.', '피부가 부드러워서 얼음이 떨어진다.'],
    }]],
  }),

  computed: {
    condition() {
      return this.$store.state.data.experimentType;
    },
    isImageType() {
      let check = false;
      if (this.problems[this.selectProblem][this.now].answer.indexOf('.png') > 0) {
        check = true;
      } else {
        check = false;
      }
      return check;
    },
    selectProblem() {
      let num = 0;
      if (this.condition === '1' || this.condition === '2') {
        num = 0;
      } else if (this.condition === '3' || this.condition === '4') {
        num = 1;
      }
      return num;
    },
  },

  mounted() {
    if ((this.condition === '3' || this.condition === '4') && this.now === 0) {
      this.text = '내가 얼음을 만졌는데 손에 달라붙었어! 왜 붙었는지 잘 모르겠어.';
      this.isProblem = false;
    }
    this.preloadImg();
    this.initTeacherImg();
    this.initAudio();
    this.countUpTimer();
  },

  methods: {
    ...mapMutations([
      'updateFields',
    ]),

    submit() {
      this.updateFields({ quizTime: this.countUp, quizfailCount: this.failCount });
      this.$router.push({ name: 'Game' });
    },

    initTeacherImg() {
      if (this.condition === '1') {
        this.teacherImg = require('@/assets/img/퀴즈선생님.png');
      } else if (this.condition === '2') {
        this.teacherImg = require('@/assets/img/유아선생님.jpg');
      } else if (this.condition === '3' || this.condition === '4') {
        this.teacherImg = require('@/assets/img/유아.png');
      }
    },

    initAudio() {
      if (this.condition === '1') {
        this.playAudio('1번-선생님', () => {
          this.disabled = false;
        });
      } else if (this.condition === '2') {
        this.playAudio('1번-유아선생님', () => {
          this.disabled = false;
        });
      } else if (this.condition === '3' || this.condition === '4') {
        this.playAudio('1번-유아', () => {
          this.disabled = false;
          this.isProblem = true;
        });
      }
    },

    preloadImg() {
      for (let i = 0; i < this.imgs.length; i += 1) {
        const img = new Image();
        img.src = this.imgs[i];
      }
      console.log('preloaded');
    },

    numbering(num) {
      let str = '';
      switch (num) {
        case 1:
          str = '①';
          break;
        case 2:
          str = '②';
          break;
        case 3:
          str = '③';
          break;
        default:
          str = '';
      }
      return str;
    },

    checkAnswer(problem, item) {
      if (problem.answer === item) {
        this.blind = false;
        this.correct = true;
        if (this.condition === '1') {
          this.playAudio('맞았을 때(딩동댕)', () => {
            this.playAudio(`${this.now + 1}번-선생님(정답)`, () => {
              this.next();
            });
          });
        } else if (this.condition === '2') {
          this.playAudio('맞았을 때(딩동댕)', () => {
            this.playAudio(`${this.now + 1}번-유아선생님(정답)`, () => {
              this.next();
            });
          });
        } else if (this.condition === '3' || this.condition === '4') {
          if (this.now === 0) {
            this.text = '아하! 손의 물기 때문이구나~ 알려줘서 고마워!';
            this.isProblem = false;
          }
          this.playAudio('맞았을 때(딩동댕)', () => {
            this.playAudio(`${this.now + 1}번-유아(정답)`, () => {
              this.next();
            });
          });
        }
      } else if (problem.answer !== item) {
        if (!this.failCount[`quiz${this.now + 1}`]) this.failCount[`quiz${this.now + 1}`] = 0;
        this.$set(this.failCount, `quiz${this.now + 1}`, this.failCount[`quiz${this.now + 1}`] + 1);
        if (this.condition === '1') {
          this.playAudio('틀렸을 때-선생님', () => {
            this.disabled = false;
          });
        } else if (this.condition === '2') {
          this.playAudio('틀렸을 때-유아선생님', () => {
            this.disabled = false;
          });
        } else if (this.condition === '3' || this.condition === '4') {
          this.playAudio('틀렸을 때-유아', () => {
            this.disabled = false;
          });
        }
      }
    },

    next() {
      setTimeout(() => {
        if (this.now + 1 < this.problems[this.selectProblem].length) {
          this.blind = true;
          this.correct = false;
          this.isStarted = false;
          this.now += 1;
          this.disabled = true;
          if (this.condition === '1') {
            this.playAudio(`${this.now + 1}번-선생님`, () => {
              this.disabled = false;
            });
          } else if (this.condition === '2') {
            this.playAudio(`${this.now + 1}번-유아선생님`, () => {
              this.disabled = false;
            });
          } else if (this.condition === '3' || this.condition === '4') {
            this.isProblem = true;
            this.playAudio(`${this.now + 1}번-유아`, () => {
              this.disabled = false;
            });
          }
        } else if (this.now + 1 === this.problems[this.selectProblem].length) {
          this.isComplete = true;
        }
      }, 1000);
    },

    playAudio(file, callback = () => {}) {
      this.file = file;

      const audio = document.getElementById(this.file);
      try {
        audio.play();
        audio.onplaying = () => {
          this.isStarted = true;
          this.isPlaying = true;
          this.disabled = true;
          this.checkAudioTime(audio);
        };
        audio.onended = () => {
          callback();
          this.isPlaying = false;
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

    checkAudioTime(audio) {
      const { currentTime } = audio;
      if (this.isPlaying && !this.isProblem) {
        setTimeout(() => {
          if (currentTime > 7) {
            this.isProblem = true;
          }
          this.checkAudioTime(audio);
        }, 100);
      }
    },

    countUpTimer() {
      if (this.$router.history.current.name === 'Quiz') {
        setTimeout(() => {
          if (!this.countUp[`quiz${this.now + 1}`]) this.countUp[`quiz${this.now + 1}`] = 0;
          // this.countUp[this.selectedImg] = ((this.countUp[this.selectedImg] * 10) + 1) / 10;
          this.$set(this.countUp, this.now,
            ((this.countUp[`quiz${this.now + 1}`] * 10) + 1) / 10);
          this.watched = Object.keys(this.countUp).length;
          this.countUpTimer();
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
  -webkit-box-orient: horizontal;
  -ms-flex-direction: row;
}
.column {
  display: flex;
  flex-direction: column;
  -webkit-box-orient: vertical;
  -ms-flex-direction: column;
  width: 50%;
  margin: 10px auto;
}
.quiz_img {
  margin: 10px auto;
  text-align: center;
}
.quiz_title {
  margin: 10px 20px 0px 20px;
  width: 80%;
  font-size: 4.2vw !important;
  line-height: 60px;
  color: orange;
  text-align: center;
}
.quiz_content {
  margin: 0px 20px 2px 20px;
  width: 80%;
  word-break: normal;
  font-size: 4vw !important;
  line-height: 35px;
  text-align: center;
}
.blind {
  background-color: orange;
  color: orange;
  border: 3px solid rgb(233, 156, 14);
  border-radius: 5px;
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
}
.circle {
  background-image: url('~@/assets/img/동그라미.png');
  background-position: center;
  background-size: 150px;
}
.check {
  background-image: url('~@/assets/img/체크.png');
  background-position-x: 13px;
  background-color: yellow;
}
.answer {
  background-color: yellow;
}

.answerlist {
  margin-top: 3px;
  font-size: 3vw;
}

.imgbtn {
  height: 60px !important;
}

.v-btn:before {
  background-color: transparent !important;
}
</style>
