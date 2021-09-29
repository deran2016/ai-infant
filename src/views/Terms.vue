<template>
  <v-card
    class="mx-auto"
    max-width="750"
    outlined
  >
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">
          1.1 실험 참여 동의
        </div>
        <v-list-item-title class="text-h5 mb-1 text-center">
          실험 참여 동의
        </v-list-item-title>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-text-field
        v-model="childID"
        label="유아 이름"
      />
      <v-text-field
        v-model="childAge"
        label="유아 나이"
        type="number"
      />
      <v-text-field
        v-model="parentID"
        label="학습을 함께하시는 부모님 성함"
      />
      <v-text-field
        v-model="parentPhone"
        label="부모님 연락처(ex.010-1234-1234)"
      />
    </v-card-text>

    <v-card-actions>
      <v-btn
        block
        color="primary"
        :disabled="!agreedAll"
        @click="submit"
      >
        다음
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
/* eslint-disable import/no-unresolved */
import { mapMutations } from 'vuex';

export default {
  data: () => ({
    childID: '',
    childAge: '',
    parentID: '',
    parentPhone: '',
  }),

  computed: {
    agreedAll() {
      return (this.childID && this.childID.length >= 2)
        && (this.childAge)
        && (this.parentID && this.parentID.length >= 2)
        && (this.parentPhone && this.parentPhone.length >= 8);
    },
  },

  mounted() {
  },

  methods: {
    ...mapMutations([
      'updateFields',
    ]),

    submit() {
      this.updateFields(this.$data);
      this.$router.push({ name: 'Intro' });
    },
  },
};
</script>

<style lang="scss">
.terms {
  height: 150px;
  overflow-y: auto;
  border: 1px solid grey;
}
</style>
