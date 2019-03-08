<style lang="less">
  @import './login.less';
</style>
<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <!-- <login-form @on-success-valid="handleSubmit"></login-form> -->
          <Form ref="loginForm" :model="form" :rules="rules" @keydown.enter.native="handleSubmit">
            <FormItem prop="userName">
              <Input v-model="form.userName" placeholder="请输入用户名">
                <span slot="prepend">
                  <Icon :size="16" type="ios-person"></Icon>
                </span>
              </Input>
            </FormItem>
            <FormItem prop="password">
              <Input type="password" v-model="form.password" placeholder="请输入密码">
                <span slot="prepend">
                  <Icon :size="14" type="md-lock"></Icon>
                </span>
              </Input>
            </FormItem>
            <FormItem>
              <Button @click="handleSubmit" type="primary" long>登录</Button>
            </FormItem>
          </Form>
        </div>
      </Card>
    </div>
  </div>
</template>

<script>
import { login } from '@/api/login'
import { setToken, getToken } from '@/libs/util'
export default {
  data () {
    return {
      form: {
        userName: 'super_admin',
        password: ''
      },
      rules: {
        userName: [{
          required: true, message: '账户不能为空', trigger: 'blur'
        }],
        password: [{
          required: true, message: '密码不能为空', maxength: 16, trigger: 'blur'
        }]
      }
    }
  },
  methods: {
    handleSubmit () {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          login({
            userName: this.form.userName,
            password: this.form.password
          }).then(res => {
            this.$store.commit('setToken', res.data.token)
            this.$router.push({
              name: this.$config.homeName
            })
          })
        }
      })
    }
  }
}
</script>

<style>

</style>
