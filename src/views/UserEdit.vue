<template>
  <div class="container py-5">
    <!-- UserForm -->
    <UserForm
      :initial-profile="profile"
      @after-submit="handleAfterSubmit"
    />
  </div>
</template>

<script>
import UserForm from '../components/UserForm'
const dummyUser = {
  profile: {
    id: 1,
    name: 'root',
    email: 'root@example.com',
    password: '$2a$10$J9pLpJJ1Tzfe/ZcjdYwXdumyh.3F5E.w/HTxRcH./cl3azhgekgQe',
    isAdmin: true,
    image: 'https://i.imgur.com/58ImzMM.png'
  }
}

export default {
  components: {
    UserForm
  },
  data() {
    return {
      profile: {
        id: -1,
        name: '',
        email: '',
        password: '',
        isAdmin: false,
        image: ''
      }
    }
  },
  created() {
    const { id } = this.$route.params
    this.fetchUser(id)
  },
  methods: {
    fetchUser(id) {
      console.log('fetchUser id: ', id)
      this.profile = dummyUser.profile
    },
    handleAfterSubmit(formData) {
      const { id } = this.$route.params
      for (const [name, value] of formData.entries()) {
        console.log(name + ': ' + value)
      }
      this.$router.push(`/users/${id}`)
    }
  }
}
</script>
