<template>
  <div class="box">
    <!-- 言い訳の入力フォームや各種ボタン -->
    <h1 class="pb-2 has-text-weight-bold">新しい言い訳を追加</h1>
    <div class="field is-grouped">
      <div class="control">
        <input
          v-model="excuse"
          class="input is-primary is-small"
          placeholder="例）明日から本気出す"
        />
      </div>
      <div class="control">
        <button
          class="button is-primary is-small has-text-weight-bold"
          @click.prevent="makeExcuse"
        >
          言い訳をする
        </button>
        <button
          class="button is-light is-primary is-small has-text-weight-bold"
          @click.prevent="clearExcuse"
        >
          言い訳を心にしまう
        </button>
      </div>
    </div>
    <!-- 言い訳内容を表示する -->
    <h1 class="pb-2 has-text-weight-bold">言い訳リスト</h1>
    <button
      class="button is-small has-text-weight-bold"
      @click.prevent="displayExcuses"
    >
      {{ buttonMessage }}
    </button>
    <table v-if="$accessor.excuses.excusesDisplay" class="table is-hoverable">
      <thead>
        <tr>
          <th class="nowrap">ID</th>
          <th class="nowrap">言い訳</th>
          <th v-if="$accessor.users.admin" class="nowrap"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(anexcuse, index) in excusesArray" :key="index">
          <td>{{ anexcuse.excuse_id }}</td>
          <td>{{ anexcuse.excuse }}</td>
          <td>
            <button
              v-if="$accessor.users.admin"
              class="button is-light is-small has-text-weight-bold"
              @click.prevent="deleteExcuse(anexcuse.excuse_id)"
            >
              削除
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      excuse: '',
      excuses: this.$accessor.excuses.excuses,
    };
  },
  computed: {
    excusesArray() {
      return this.$accessor.excuses.excuses;
    },
    buttonMessage() {
      if (this.$accessor.excuses.excusesDisplay === true) {
        return 'みんなの言い訳を閉じる';
      } else {
        return 'みんなの言い訳を見る';
      }
    },
  },
  /* eslint-disable */
  created: function () {
    this.$accessor.excuses.setExcuses();
  },
  methods: {
    clearExcuse() {
      this.excuse = '';
    },
    makeExcuse() {
      this.$accessor.excuses.pushExcuse(this.excuse);
      this.excuse = '';
    },
    displayExcuses() {
      this.$accessor.excuses.toggleExcuseDisplay();
    },
    deleteExcuse(id: number) {
      this.$accessor.excuses.deleteExcuse(id);
    },
  },
});
</script>

<style scoped>
.box {
  overflow: scroll;
}
</style>
