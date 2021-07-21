<template>
  <transition name="modal">
    <div class="overlay" @click.self="$emit('close')">
      <div class="modal_window">
        <!-- 店舗情報更新 -->
        <h2 id="title">情報更新</h2>
        <!-- 店舗情報 -->
        <table>
          <tr>
            <th>アプリ名:</th>
            <td>
              <input type="text" v-model="val.name" />
            </td>
          </tr>
          <tr>
            <!-- 更新前画像 -->
            <th>画像:</th>
            <td>
              <input
                type="text"
                @change="onFileChange"
                accept="image/*"
                class="image_input"
                v-model="val.image"
              />
            </td>
          </tr>
          <tr>
            <th>住所:</th>
            <td>
              <input
                type="text"
                v-model="val.address"
              />
            </td>
          </tr>
          <tr>
            <th>都市名(天気用):</th>
            <td>
              <input type="text" v-model="val.city_name" />
            </td>
          </tr>
          <tr>
            <th>ツイッターアカウント:</th>
            <td>
              <input type="text" v-model="val.tw_account" />
            </td>
          </tr>
          <tr>
            <th>開催初日:</th>
            <td>
              <input type="text" v-model="val.event_start_date" />
            </td>
          </tr>
          <tr>
            <th>開催2日目:</th>
            <td>
              <input type="text" v-model="val.event_2_date" />
            </td>
          </tr>
          <tr>
            <th>開催3日目:</th>
            <td>
              <input
                type="text"
                v-model="val.event_3_date"
              />
            </td>
          </tr>
          <tr>
            <th>開催4日目:</th>
            <td>
              <input
                type="text"
                v-model="val.event_4_date"
              />
            </td>
          </tr>
          <tr>
            <th>開催5日目:</th>
            <td>
              <input
                type="text"
                v-model="val.event_last_date"
              />
            </td>
          </tr>
        </table>

        <button class="button update_button" @click="updatePtf">
          <p v-if="loading">情報更新</p>
          <vue-loading
            type="barsCylon"
            color="#fff"
            v-else
            class="loading"
          ></vue-loading>
        </button>
      </div>
    </div>
  </transition>
</template>

<script>
import axios from "axios";
import { VueLoading } from "vue-loading-template";
export default {
  props: ["val"],
  data() {
    return {
      selectImage: false,
      // preview: "",
      // file: "",
      image_loading: true,
      loading: true,
    };
  },
  components: {
    VueLoading,
  },
  methods: {
    // 画像アップデート
    // imageUpdate() {
    //   this.image_loading = false;
    //   const formData = new FormData();
    //   formData.append("image", this.file);
    //   formData.append("id", this.val.id);
    //   axios
    //     .post(
    //       "https://yukinas-portfolio.herokuapp.com/api/portfolio/image",
    //       formData
    //     )
    //     .then(() => {
    //       alert("画像を更新しました。");
    //       this.image_loading = true;
    //     })
    //     .catch(() => {
    //       alert("画像を更新できませんでした。お手数ですが再度お試しください。");
    //       this.image_loading = true;
    //     });
    // },
    // 情報更新
    updatePtf() {
      this.loading = false;
      axios
        .put("https://feslive.herokuapp.com/api/event", {
          id: this.val.id,
          name: this.val.name,
          tw_account: this.val.tw_account,
          image: this.val.image,
          address: this.val.address,
          city_name: this.val.city_name,
          event_start_date: this.val.event_start_date,
          event_2_date: this.val.event_2_date,
          event_3_date: this.val.event_3_date,
          event_4_date: this.val.event_4_date,
          event_last_date: this.val.event_last_date,
        })
        .then((response) => {
          console.log(response);
          alert("情報を更新しました。");
          this.$emit("input", false);
          this.$router.go({
            path: this.$router.currentRoute.path,
            force: true,
          });
          this.loading = true;
        })
        .catch((err) => {
          console.log(err);
          alert("更新できませんでした。お手数ですが再度お試しください。");
          this.loading = true;
        });
    },
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
        this.selectImage = true;
      }),
        reader.readAsDataURL(event.target.files[0]);
    },
    reset() {
      (this.preview = null),
        (this.file = null),
        (this.$el.querySelector('input[type="file"]').value = null);
      this.selectImage = false;
    },
  },
};
</script>

<style scoped>
/* ====================
      情報更新
==================== */
.overlay {
  z-index: 1;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal_window {
  text-align: left;
  z-index: 2;
  width: 80%;
  padding: 10px;
  background: #fff;
  position: relative;
}
/* ====================
        テーブル
==================== */
table {
  width: 100%;
  margin-bottom: 60px;
}
tr {
  border: 1px solid #c2c2c2;
}
th {
  width: 25%;
  font-size: 16px;
  background-color: rgb(212, 208, 201);
  padding: 5px;
}
td {
  width: 60%;
  padding: 5px;
}
.update_button {
  background-color: rgb(2, 223, 186);
}

input,
select {
  width: 100%;
  box-sizing: border-box;
  padding: 5px;
  font-size: 18px;
}
.update_button {
  width: 100%;
  height: 45px;
  background-color: rgba(0, 0, 0, 0.8);
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 10px;
}
span {
  margin-top: 6%;
  font-size: 25px;
  font-weight: bold;
}
</style>
