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
  </div>
</template>

<script>
export default {
  data: () => ({
    annualzone: [
      '나이테-선생님',
    ],
    ice: [
      '얼음-선생님',
      '얼음-유아선생님',
      '얼음-유아선생님 with Peer',
      '얼음-선생님 with Peer',
    ],
    leaf: [
      '잎 모양-선생님',
    ],
  }),

  computed: {
    condition() {
      return this.$store.state.data.experimentType;
    },
  },
};
</script>
