<template>
  <div class="store_setting">
    <!-- イベント情報登録 -->
    <Management />
    <h2>イベント情報登録</h2>
    <table>
      <tr>
        <th>名前:</th>
        <td><input type="text" v-model="name" /></td>
      </tr>
      <tr>
        <th>住所:</th>
        <td>
          <textarea
            type="text"
            size="20"
            rows="5"
            cols="30"
            wrap="”soft”"
            v-model="detail"
          ></textarea>
        </td>
      </tr>
      <tr>
        <th>画像:</th>
        <td>
          <input type="file" @change="onFileChange" accept="image/*" />
          <div class="image" v-if="preview">
            <img :src="preview" />
          </div>
        </td>
      </tr>
      <tr>
        <th>都市名(天気用):</th>
        <td>
          <input type="text" v-model="city_name" />
        </td>
      </tr>
      <tr>
        <th>ツイッターアカウント:</th>
        <td>
          <input type="text" v-model="tw_account" />
        </td>
      </tr>
      <tr>
        <th>開催初日:</th>
        <td>
          <input type="text" v-model="event_start_date" />
        </td>
      </tr>
      <tr>
        <th>開催2日目:</th>
        <td>
          <input type="text" v-model="event_2_date" />
        </td>
      </tr>
      <tr>
        <th>開催3日目:</th>
        <td>
          <textarea
            type="text"
            size="20"
            rows="5"
            cols="30"
            wrap="”soft”"
            v-model="event_3_date"
          ></textarea>
        </td>
      </tr>
      <tr>
        <th>開催4日目:</th>
        <td>
          <textarea
            type="text"
            size="20"
            rows="5"
            cols="30"
            wrap="”soft”"
            v-model="event_4_date"
          ></textarea>
        </td>
      </tr>
      <tr>
        <th>開催最終日:</th>
        <td>
          <textarea
            type="text"
            size="20"
            rows="5"
            cols="30"
            wrap="”soft”"
            v-model="event_last_date"
          ></textarea>
        </td>
      </tr>
    </table>

    <button class="button" @click="createPtf">
      <p v-if="loading">登録</p>
      <vue-loading
        type="barsCylon"
        color="#fff"
        v-else
        class="loading"
      ></vue-loading>
    </button>
  </div>
</template>

<script>
import axios from "axios";
import Management from "../components/Management.vue";
import { VueLoading } from "vue-loading-template";
export default {
  data() {
    return {
      preview: "",
      file: "",
      name: "",
      detail: "",
      city_name: "",
      tw_account: "",
      event_start_date: "",
      event_2_date: "",
      event_3_date: "",
      event_last_date: "",
      loading: true,
    };
  },
  components: {
    Management,
    VueLoading,
  },
  methods: {
    // 画像プレビュー
    onFileChange(event) {
      this.file = event.target.files[0];
      // 何も選択されていなかったら処理中断
      if (event.target.files.length === 0) {
        this.reset();
        return false;
      }
      const reader = new FileReader();
      (reader.onload = (e) => {
        this.preview = e.target.result;
      }),
        reader.readAsDataURL(event.target.files[0]);
    },
    reset() {
      (this.preview = null),
        (this.file = null),
        (this.$el.querySelector('input[type="file"]').value = null);
    },

    // イベント登録
    createPtf() {
      this.loading = false;
      const formData = new FormData();
      formData.append("image", this.file);
      formData.append("name", this.name);
      formData.append("detail", this.detail);
      formData.append("created", this.created);
      formData.append("url", this.url);
      formData.append("github_front", this.github_front);
      formData.append("github_api", this.github_api);
      formData.append("difficulties", this.difficulties);
      formData.append("solutions", this.solutions);
      axios
        .post("https://yukinas-portfolio.herokuapp.com/api/portfolio", formData)
        .then((response) => {
          console.log(response);
          alert("登録しました。");
          this.loading = true;
        })
        .catch((error) => {
          console.log(error);
          alert("登録できませんでした。お手数ですが、再度お試しください。");
          this.loading = true;
        });
    },
  },
};
</script>

<style scoped>
/* ====================
      イベント登録
==================== */
.store_setting {
  width: 80%;
  margin-left: 200px;
}
h2 {
  font-size: 25px;
}
table {
  width: 100%;
  margin-top: 20px;
  text-align: left;
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.4);
}
tr {
  border: 1px solid #c2c2c2;
}
th {
  width: 15%;
  font-size: 18px;
  padding: 5px;
  background-color: rgb(212, 208, 201);
}
td {
  width: 70%;
  padding: 10px 5px;
}
td:nth-of-type(1) {
  padding-bottom: 30px;
}
input,
textarea,
select {
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
  font-size: 18px;
}
textarea {
  display: block;
}
.image {
  width: 100%;
  position: relative;
  overflow: hidden;
  padding-top: 60%;
  margin: 10px 0;
}
.image img {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.button {
  margin: 20px 0 20px 50%;
  width: 100px;
  height: 35px;
  font-weight: bold;
  transform: translate(-50%);
  background-color: rgb(108, 209, 115);
}
.loading {
  margin-right: 20px;
}
</style>
