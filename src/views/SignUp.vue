<template>
  <div class="container py-5">
    <form
      class="w-100"
      :disabled="isProcessing"
      @submit.prevent.stop="handleSubmit"
    >
      <div class="text-center mb-4">
        <h1 class="h3 mb-3 font-weight-normal">
          Sign Up
        </h1>
      </div>

      <div class="form-label-group mb-2">
        <label for="name">Name</label>
        <input
          id="name"
          v-model="name"
          name="name"
          type="text"
          class="form-control"
          placeholder="name"
          required
          autofocus
        >
      </div>

      <div class="form-label-group mb-2">
        <label for="email">Email</label>
        <input
          id="email"
          v-model="email"
          name="email"
          type="email"
          class="form-control"
          placeholder="email"
          required
        >
      </div>

      <div class="form-label-group mb-3">
        <label for="password">Password</label>
        <input
          id="password"
          v-model="password"
          name="password"
          type="password"
          class="form-control"
          placeholder="Password"
          required
        >
      </div>

      <div class="form-label-group mb-3">
        <label for="password-check">Password Check</label>
        <input
          id="password-check"
          v-model="passwordCheck"
          name="passwordCheck"
          type="password"
          class="form-control"
          placeholder="Password"
          required
        >
      </div>

      <button
        class="btn btn-lg btn-primary btn-block mb-3"
        type="submit"
      >
        {{ isProcessing ? '註冊中...' : 'Submit' }}
      </button>

      <div class="text-center mb-3">
        <p>
          <router-link to="/signin">
            Sign In
          </router-link>
        </p>
      </div>

      <p class="mt-5 mb-3 text-muted text-center">
        &copy; 2017-2018
      </p>
    </form>
  </div>
</template>

<script>
import authorizationAPI from '../apis/authorization'
import { Toast } from '../utils/helpers.js'
export default {
  name: 'SignUp',
  data() {
    return {
      name: '',
      email: '',
      password: '',
      passwordCheck: '',
      isProcessing: false
    }
  },
  methods: {
    async handleSubmit(e) {
      try {
        this.Processing = true
        if (!this.name || !this.email || !this.password || !this.passwordCheck) {
          Toast.fire({
            icon: 'warning',
            title: '所有欄位皆為必填'
          })
          this.Processing = false
          return
        }

        if (this.password !== this.passwordCheck) {
          Toast.fire({
            icon: 'warning',
            title: 'Password與Password Check不符'
          })
          this.Processing = false
          return
        }

        const { data, statusText } = await authorizationAPI.signUp({
          name: this.name,
          email: this.email,
          password: this.password,
          passwordCheck: this.passwordCheck
        })

        // console.log觀察，信箱重複時statusText: 'OK'，但back-end回傳的data狀態為 'error'，且data.message存放著back-end的錯誤訊息
        if (statusText === 'OK' && data.status !== 'success') {
          Toast.fire({
            icon: 'warning',
            title: data.message
          })
          // this.Processing = false
          // 其他錯誤，如：連線問題，丟給catch處理
          if (statusText !== 'OK') {
            throw new Error(statusText)
          }
        } else {
          Toast.fire({
            icon: 'success',
            title: '註冊成功，請登入使用'
          })
          this.$router.push('/signin')
        }
      } catch (error) {
        this.Processing = false
        Toast.fire({
          icon: 'error',
          title: '無法註冊，請稍後再試'
        })
      }
    }
  }
}
</script>
