<template>
  <div class="login-container">
    <div class="login-header">
      <span class="header-icon">传</span>DPOE商家管理后台
    </div>
    <el-form autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left" label-width="0px"
      class="card-box login-form">
      <h3 class="title">系统登录</h3>
      <el-form-item prop="email">
        <span class="svg-container">
                  <icon-svg icon-class="jiedianyoujian"></icon-svg>
                </span>
        <el-input name="email" type="text" v-model="loginForm.email" autoComplete="on" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <span class="svg-container">
                  <icon-svg icon-class="mima" ></icon-svg>
                </span>
        <el-input name="password" type="password" @keyup.enter.native="handleLogin" v-model="loginForm.password" autoComplete="on"
          placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width:100%;" :loading="loading" @click.native.prevent="handleLogin">
          登录
        </el-button>
      </el-form-item>
      <div class='tips'>admin账号为:admin@wallstreetcn.com 密码随便填</div>
      <div class='tips'>editor账号:editor@wallstreetcn.com 密码随便填</div>
    </el-form>
  </div>
</template>

<script>
    import { isWscnEmail } from '@/utils/validate';

    export default {
      name: 'login',
      data() {
        const validateEmail = (rule, value, callback) => {
          if (!isWscnEmail(value)) {
            callback(new Error('请输入正确的合法邮箱'));
          } else {
            callback();
          }
        };
        const validatePass = (rule, value, callback) => {
          if (value.length < 6) {
            callback(new Error('密码不能小于6位'));
          } else {
            callback();
          }
        };
        return {
          loginForm: {
            email: 'admin@wallstreetcn.com',
            password: '111111'
          },
          loginRules: {
            email: [
                { required: true, trigger: 'blur', validator: validateEmail }
            ],
            password: [
                { required: true, trigger: 'blur', validator: validatePass }
            ]
          },
          loading: false
        }
      },
      methods: {
        handleLogin() {
          this.$refs.loginForm.validate(valid => {
            if (valid) {
              this.loading = true;
              this.$store.dispatch('Login', this.loginForm).then(() => {
                this.loading = false;
                this.$router.push({ path: '/' });
              }).catch(() => {
                this.loading = false;
              });
            } else {
              console.log('error submit!!');
              return false;
            }
          });
        }
      }
    }
</script>

<style rel="stylesheet/scss" lang="scss">
    @import "src/styles/mixin.scss";
    .tips {
      font-size: 14px;
      color: #e4e4e4;
      margin-bottom: 5px;
    }

    .login-container {
      @include relative;
      height: 100vh;
      background-color: #2d3a4b;
      background-image: url('../../assets/login_images/login_bg.jpg');
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center center;
      input:-webkit-autofill {
        -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
        -webkit-text-fill-color: #fff !important;
      }
      input {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        padding: 12px 5px 12px 15px;
        color: #e4e4e4;
        font-size: 16px;
        height: 47px;
      }
      .el-input {
        display: inline-block;
        height: 47px;
        width: 85%;
      }
      .svg-container {
        padding: 6px 5px 6px 15px;
        color: #889aa4;
      }
      .title {
        font-size: 24px;
        font-weight: 400;
        color: #333;
        margin: 0px auto 40px auto;
        text-align: left;
        font-weight: bold;
      }
      .login-header {
        position: absolute;
        width: 100%;
        height: 80px;
        line-height: 80px;
        font-size: 26px;
        padding-left: 20%;
        background: #fff;
        color: #000;

        .header-icon {
          display: inline-block;
          width: 40px;
          height: 40px;
          line-height: 40px;
          text-align: center;
          border-radius: 10px;
          background: #000;
          font-size: 20px;
          font-weight: bold;
          color: #fff;
          vertical-align: 2px;
          margin-right: 5px;
        }
      }
      .login-form {
        position: absolute;
        /* left: 0; */
        right: 18%;
        top: 20%;
        width: 400px;
        padding: 35px 35px 15px 35px;
        background: #fff;
        /* margin: 120px auto; */
      }
      .el-form-item {
        border: 1px solid rgb(228, 228, 228);
        /* background: rgba(0, 0, 0, 0.1); */
        border-radius: 5px;
        color: #e4e4e4;
      }
      .forget-pwd {
        color: #fff;
      }
      .el-button--primary {
        background: #000;
        border-color: #000;
      }
    }
</style>
