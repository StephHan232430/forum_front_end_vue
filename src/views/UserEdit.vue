<template>
  <div class="container py-5">
    <form @submit.stop.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          id="name"
          v-model="name"
          type="text"
          class="form-control"
          name="name"
          placeholder="Enter name"
          required
        >
      </div>

      <div class="form-group">
        <label for="image">Image</label>

        <img
          v-if="image"
          :src="image"
          class="d-block img-thumbnail mb-3"
          width="200"
          height="200"
        >

        <input
          id="image"
          type="file"
          name="image"
          accept="image/*"
          class="form-control-file"
          @change="handleFileChange"
        >
      </div>

      <button
        type="submit"
        class="btn btn-primary"
      >
        送出
      </button>
    </form>
  </div>
</template>

<script>
import usersAPI from '../apis/users'
import { Toast } from '../utils/helpers'

export default {
  data() {
    return {
      id: -1,
      name: '',
      email: '',
      image: ''
    }
  },
  created() {
    const { id } = this.$route.params
    this.fetchUser(id)
  },
  methods: {
    async fetchUser(userId) {
      try {
        const { data, statusText } = await usersAPI.get({ userId })
        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        this.id = data.profile.id
        this.name = data.profile.name
        this.email = data.profile.email
        this.image = data.profile.image
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法編輯使用者資訊，請稍後再試'
        })
      }
    },
    handleFileChange(e) {
      const files = e.target.files
      if (!files.length) return
      const imageURL = window.URL.createObjectURL(files[0])
      this.image = imageURL
    },
    async handleSubmit(e) {
      try {
        const form = e.target
        const formData = new FormData(form)
        const { data, statusText } = await usersAPI.update({ userId: this.id, formData })
        if (statusText !== 'OK' || data.status !== 'success') {
          throw new Error(statusText)
        }
        this.$router.push({ name: 'user-profile', params: { id: this.id } })
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法更新使用者，請稍後再試'
        })
      }
    }
  }
}
</script>
