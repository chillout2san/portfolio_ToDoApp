<template>
  <div class="box">
    <h1 class="has-text-weight-bold">今日のタスクリスト</h1>
    <table class="table is-hoverable">
      <thead>
        <tr>
          <th class="nowrap">ID</th>
          <th class="nowrap">名前</th>
          <th class="nowrap">進捗状況</th>
          <th class="nowrap">締め切り</th>
          <th class="nowrap">アラート</th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in workingTodos" :key="index">
          <td>{{ todo.todo_id }}</td>
          <td>{{ todo.todo_name }}</td>
          <td>{{ todo.status }}</td>
          <td>{{ todo.deadline }}</td>
          <td>{{ todo.alert_function }}</td>
          <!-- 詳細ボタンを表示する -->
          <td>
            <button
              class="button is-primary is-small has-text-weight-bold"
              @click="changeDetailModal(todo.todo_id)"
            >
              詳細
            </button>
          </td>
          <!-- 非表示ボタンを表示する -->
          <td>
            <button
              class="button is-small is-light has-text-weight-bold"
              @click="disableWorking(todo.todo_id)"
            >
              非表示
            </button>
          </td>
          <!-- 詳細ボタンを押下した時のモーダルウィンドウ -->
          <div :class="todo.detail_display">
            <div
              class="modal-background"
              @click="changeDetailModal(todo.todo_id)"
            ></div>
            <div class="modal-content">
              <div class="box">
                <div class="message is-primary">
                  <div class="message-header is-size-5">
                    <p>{{ todo.todo_name }}</p>
                  </div>
                  <div class="message-body">
                    {{ todo.info }}
                  </div>
                </div>
              </div>
            </div>
            <button
              class="modal-close is-large"
              @click="changeDetailModal(todo.todo_id)"
            ></button>
          </div>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import firebase from 'plugins/firebase';

export default Vue.extend({
  data() {
    return {};
  },
  computed: {
    workingTodos(): firebase.firestore.DocumentData[] {
      return this.$accessor.todos.todos.filter((todo) => {
        return todo.working === true;
      });
    },
  },
  methods: {
    changeDetailModal(id: number) {
      this.$accessor.todos.displayDetailModal(id);
    },
    deleteTodo(id: number) {
      this.$accessor.todos.deleteTodo(id);
    },
    disableWorking(id: number) {
      this.$accessor.todos.disableWorkingDisplay(id);
    },
  },
});
</script>

<style scoped>
.nowrap {
  white-space: nowrap;
}

.box {
  overflow: scroll;
}
</style>
