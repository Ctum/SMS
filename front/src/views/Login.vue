<template>
  <div class="bg">
  <div class="login-container">
    <h1>{{ nowTerm }}学期选课系统登录</h1>
    <a-form
      id="components-form-demo-normal-login"
      :form="form"
      class="login-form"
      @submit="handleSubmit"
    >
      <a-form-item>
          <a-input
            v-decorator="[
              'userName',
              { rules: [{ required: true, message: 'Please input your username!' }] }
            ]"
            placeholder="Username"
          >
            <a-icon
              slot="prefix"
              type="user"
              style="color: rgba(0,0,0,.25)"
            />
          </a-input>
      </a-form-item>
      <a-form-item>
        <a-input
          v-decorator="[
            'password',
            { rules: [{ required: true, message: 'Please input your Password!' }] }
          ]"
          type="password"
          placeholder="Password"
        >
          <a-icon
            slot="prefix"
            type="lock"
            style="color: rgba(0,0,0,.25)"
          />
        </a-input>
      </a-form-item>
      <a-button
        type="primary"
        html-type="submit"
        class="login-form-button"
      >
        Log in
      </a-button>
    </a-form>
  </div>
  </div>
</template>

<script>
// @ is an alias to /src
import api from '@/api';
import { mapGetters } from 'vuex';

export default {
  name: 'login',
  computed: {
    ...mapGetters(['nowTerm'])
  },
  methods: {
    handleSubmit(e){
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          api.login(values).then(res => {
            const { role, userInfo } = res.data.res;
            sessionStorage.setItem('accessToken', values.userName);
            this.$store.commit('setUserInfo', {
              userInfo,
              role
            });
            this.$router.push('/');
          }).catch(() => {
            this.$message.error("用户名或密码错误");
          });
        } else {
          this.$message.error(err);
        }
      });
    }
  },
  beforeCreate() {
    this.form = this.$form.createForm(this);
  }
}
</script>

<style lang="less" scoped>

.bg {
  height: 100%;
  background-image: url('../assets/lib.jpg');
  background-repeat: no-repeat;
  background-size: 100% 100%;
}

.login-container {
  width:500px;
  margin: auto auto;
  padding-top: 240px;
  h1 {
    text-align: center;
  }
}
#components-form-demo-normal-login {
  .login-form-button {
    width: 100%;
  }
}
</style>
