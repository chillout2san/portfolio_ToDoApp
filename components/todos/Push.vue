<template>
  <form class="box">
    <h1 class="pb-2 has-text-weight-bold">新しいタスクを追加</h1>
    <div class="field">
      <label class="label has-text-weight-normal">名前</label>
      <div class="control">
        <input
          v-model="name"
          class="input is-primary is-small"
          placeholder="例）買い物"
        />
      </div>

      <label class="label has-text-weight-normal">詳細</label>
      <div class="control">
        <textarea
          v-model="info"
          class="textarea is-primary is-small"
          placeholder="例）予算は千円"
          rows="3"
        ></textarea>
      </div>

      <label class="label has-text-weight-normal">進捗状況</label>
      <div class="control">
        <div class="select is-primary is-small">
          <select v-model="status">
            <option>作業中</option>
            <option>依頼中</option>
          </select>
        </div>
      </div>

      <label class="label has-text-weight-normal">締め切り</label>
      <div class="control pb-2">
        <div class="select is-primary is-small">
          <select v-model="deadlineYear">
            <option v-for="(year, key) in years" :key="key" :value="year">
              {{ year }}
            </option>
          </select>
        </div>
        年

        <div class="select is-primary is-small">
          <select v-model="deadlineMonth">
            <option v-for="(month, key) in monthes" :key="key" :value="month">
              {{ month }}
            </option>
          </select>
        </div>
        月

        <div class="select is-primary is-small">
          <select v-model="deadlineDay">
            <option v-for="(day, key) in days" :key="key" :value="day">
              {{ day }}
            </option>
          </select>
        </div>
        日
      </div>

      <label class="label has-text-weight-normal">アラート</label>
      <div class="control">
        <label class="radio">
          <input
            type="radio"
            name="answer"
            @click="change_alert_function('有効')"
          />
          有効
        </label>
        <label class="radio">
          <input
            type="radio"
            name="answer"
            @click="change_alert_function('無効')"
          />
          無効
        </label>
      </div>
    </div>

    <div class="field is-grouped">
      <div class="control">
        <button
          class="button is-primary is-small has-text-weight-bold"
          @click.prevent="
            pushTodo(name, info, status, deadline, alert_function)
          "
        >
          登録
        </button>
      </div>
      <div class="control">
        <button
          class="button is-light is-primary is-small has-text-weight-bold"
          @click.prevent="clearForm"
        >
          入力内容をクリア
        </button>
      </div>
    </div>
  </form>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      name: '',
      info: '',
      status: '',
      deadlineYear: '',
      deadlineMonth: '',
      deadlineDay: '',
      alert_function: '',
    };
  },
  computed: {
    deadline(): string {
      if (
        this.deadlineYear === '' ||
        this.deadlineMonth === '' ||
        this.deadlineDay === ''
      ) {
        return '';
      } else {
        return (
          this.deadlineYear + '/' + this.deadlineMonth + '/' + this.deadlineDay
        );
      }
    },
    years(): string[] {
      let presentYear = new Date().getFullYear();
      const years = [];
      for (let i = 0; i < 5; i++) {
        years.push(presentYear.toString());
        presentYear++;
      }
      return years;
    },
    monthes(): string[] {
      const monthes = [];
      for (let i = 1; i < 13; i++) {
        monthes.push(i.toString());
      }
      return monthes;
    },
    days(): string[] {
      const days = [];
      for (let i = 1; i < 32; i++) {
        days.push(i.toString());
      }
      return days;
    },
  },
  methods: {
    change_alert_function(boolean: string) {
      this.alert_function = boolean;
    },
    clearForm() {
      this.name = '';
      this.info = '';
      this.status = '';
      this.deadlineYear = '';
      this.deadlineMonth = '';
      this.deadlineDay = '';
      this.deadline = '';
    },
    pushTodo(
      name: string,
      info: string,
      status: string,
      deadline: string,
      alertFunction: string
    ) {
      this.$accessor.todos.pushTodo([
        name,
        info,
        status,
        deadline,
        alertFunction,
      ]);
      this.name = '';
      this.info = '';
      this.status = '';
      this.deadlineYear = '';
      this.deadlineMonth = '';
      this.deadlineDay = '';
      this.deadline = '';
    },
  },
});
</script>

<style scoped>
.box {
  overflow: scroll;
}
</style>
