<template>
  <div>
    <h1>生徒詳細</h1>
    <v-simple-table class="mb-4">
      <template v-slot:default>
        <tbody>
          <tr>
            <th class="text-left">氏名</th>
            <td>{{ student.name }}</td>
          </tr>
          <tr>
            <th class="text-left">クラス</th>
            <td>{{ student.classroom }}</td>
          </tr>
          <tr>
            <th class="text-left">住所</th>
            <td>{{ student.address }}</td>
          </tr>
          <tr>
            <th class="text-left">部活動</th>
            <td>{{ student.club }}</td>
          </tr>
          <tr>
            <th class="text-left">性別</th>
            <td>{{ student.sex | sex }}</td>
          </tr>
          <tr>
            <th class="text-left">身長</th>
            <td>{{ student.height }} cm</td>
          </tr>
          <tr>
            <th class="text-left">体重</th>
            <td>{{ student.weight }} kg</td>
          </tr>
          <tr>
            <th class="text-left">年齢</th>
            <td>{{ student.age }} 歳</td>
          </tr>
          <tr>
            <th class="text-left">Tel</th>
            <td>{{ student.tel }}</td>
          </tr>
          <tr>
            <th class="text-left">Email</th>
            <td>{{ student.email }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <div>
      <!-- <v-btn color="info" class="mr-4">編集</v-btn> -->
      <SaveStudent mode="update" color="info" :student="student">
        編集
      </SaveStudent>
      <v-btn @click="destroy" color="error">削除</v-btn>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { createNamespacedHelpers } from "vuex";
import SaveStudent from "@/components/SaveStudent.vue";
const { mapGetters, mapActions } = createNamespacedHelpers("student");
export default Vue.extend({
  components: { SaveStudent },
  filters: {
    sex: function(value: string) {
      switch (value) {
        case "0":
          return "未回答";
        case "1":
          return "男";
        case "2":
          return "女";
        case "9":
          return "その他";
        default:
          return "未回答";
      }
    }
  },
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  computed: {
    ...mapGetters(["student"])
  },
  created() {
    this.getStudent(this.id);
  },
  methods: {
    ...mapActions(["getStudent", "deleteStudent"]),
    destroy() {
      if (confirm("削除しますか？")) {
        this.deleteStudent(this.id);
        this.$router.push({ name: "Students" });
      }
    }
  }
});
</script>
