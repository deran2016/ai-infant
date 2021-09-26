<template>
  <div class="mediaList">
    <!-- 나이테 -->
    <div>
      <audio
        v-for="(file, key) in annualzone"
        :id="file"
        :key="key"
        :src="require(`@/assets/나이테/${file}.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 얼음 -->
    <div>
      <audio
        v-for="(file, key) in ice"
        :id="file"
        :key="key"
        :src="require(`@/assets/얼음/${file}.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 잎 -->
    <div>
      <audio
        v-for="(file, key) in leaf"
        :id="file"
        :key="key"
        :src="require(`@/assets/잎/${file}.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 퀴즈-선생님 -->
    <div>
      <audio
        v-for="(file, key) in quiz"
        :id="file + '-선생님'"
        :key="key"
        :src="require(`@/assets/quiz/${file}-선생님.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 퀴즈-선생님(정답) -->
    <div>
      <audio
        v-for="(file, key) in quiz"
        :id="file + '-선생님(정답)'"
        :key="key"
        :src="require(`@/assets/quiz/${file}-선생님(정답).mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 퀴즈-유아 -->
    <div>
      <audio
        v-for="(file, key) in quiz"
        :id="file + '-유아'"
        :key="key"
        :src="require(`@/assets/quiz/${file}-유아.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 퀴즈-유아(정답) -->
    <div>
      <audio
        v-for="(file, key) in quiz"
        :id="file + '-유아(정답)'"
        :key="key"
        :src="require(`@/assets/quiz/${file}-유아(정답).mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 퀴즈-유아선생님 -->
    <div>
      <audio
        v-for="(file, key) in quiz"
        :id="file + '-유아선생님'"
        :key="key"
        :src="require(`@/assets/quiz/${file}-유아선생님.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 퀴즈-유아선생님(정답) -->
    <div>
      <audio
        v-for="(file, key) in quiz"
        :id="file + '-유아선생님(정답)'"
        :key="key"
        :src="require(`@/assets/quiz/${file}-유아선생님(정답).mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>

    <!-- 효과음 -->
    <div>
      <audio
        v-for="(file, key) in effect"
        :id="file"
        :key="key"
        :src="require(`@/assets/quiz/${file}.mp3`)"
        preload="auto"
      >
        <p>Your browser does not support the <code>audio</code> element.</p>
      </audio>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    annualzone: [
      '나이테-선생님',
      '나이테-유아선생님',
      '나이테-선생님 with Peer',
      '나이테-유아선생님 with Peer',
    ],
    ice: [
      '얼음-선생님',
      '얼음-유아선생님',
      '얼음-유아선생님 with Peer',
      '얼음-선생님 with Peer',
    ],
    leaf: [
      '잎 모양-선생님',
      '잎 모양-유아선생님',
      '잎 모양-선생님 with Peer',
      '잎 모양-유아선생님 with Peer',
    ],
    quiz: [
      '1번',
      '2번',
      '3번',
      '4번',
      '5번',
    ],
    effect: [
      '맞았을 때(딩동댕)',
      '틀렸을 때-선생님',
      '틀렸을 때-유아',
      '틀렸을 때-유아선생님',
    ],
  }),

  computed: {
    condition() {
      return this.$store.state.data.experimentType;
    },
  },
};
</script>
