<template>
  <div class="register">
    <van-nav-bar
      title="用户注册"
      left-text="返回"
      left-arrow
      @click-left="goBack"
    />
    <div class="register-panel">
      <van-field
        v-model="userName"
        label="用户名"
        placeholder="请输入用户名"
        required
        clearable
        :error-message="userNameErrorMsg"
      />
      <van-field
        v-model="password"
        label="密码"
        type="password"
        placeholder="请输入密码"
        required
        clearable
        :error-message="passworErrorMsg"
      />
      <div class="register-button">
        <van-button
          type="primary"
          @click="registerAction"
          size="large"
          :loading="openLoading"
          >注册</van-button
        >
      </div>
    </div>
  </div>
</template>

<script>
import url from "@/serviceAPI.config.js";
import { Toast } from "vant";
export default {
  data() {
    return {
      userName: "",
      password: "",
      openLoading: false, //是否开启用户注册的loading状态
      userNameErrorMsg: "", //用户名出错提示
      passworErrorMsg: "" //密码出错提示
    };
  },
  methods: {
    goBack() {
      this.$router.go(-1);
    },
    registerAction() {
      this.checkForm() && this.axiosRegister();
    },
    axiosRegister() {
      this.openLoading = true;
      this.axios({
        url: url.registerUser,
        method: "post",
        data: {
          userName: this.userName,
          password: this.password
        }
      }).then(res => {
        console.log(res);
        if (res.data.code === 200) {
          Toast.success(res.data.msg);
          this.$router.push("/");
        } else {
          this.openLoading = false;
          Toast.fail("注册失败");
        }
        console.log(res.data.msg);
      });
    },
    //表单验证方法
    checkForm() {
      let isOk = true;
      if (this.userName.length < 5) {
        this.userNameErrorMsg = "用户名不能小于5位";
        isOk = false;
      } else {
        this.userNameErrorMsg = "";
      }
      if (this.password.length < 6) {
        isOk = false;
        this.passworErrorMsg = "密码不能小于6位";
      } else {
        this.userNameErrorMsg = "";
      }
      return isOk;
    }
  }
};
</script>

<style lang="scss" scoped>
.register-panel {
  width: 96%;
  border-radius: 5px;
  margin: 20px auto;
  padding-bottom: 80px;
}
.register-button {
  padding-top: 10px;
}
</style>
