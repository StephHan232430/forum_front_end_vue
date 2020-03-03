<template>
  <div class="container py-5">
    <!-- AdminNav Component -->
    <AdminNav />

    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th scope="col">
            #
          </th>
          <th scope="col">
            Email
          </th>
          <th scope="col">
            Role
          </th>
          <th
            scope="col"
            width="140"
          >
            Action
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="user in users"
          :key="user.id"
        >
          <th scope="row">
            {{ user.id }}
          </th>
          <td>
            {{ user.email }}
          </td>
          <td v-show="user.isAdmin">
            admin
          </td>
          <td v-show="!user.isAdmin">
            User
          </td>
          <td v-if="!user.isCurrentUser">
            <button
              v-show="user.isAdmin"
              type="button"
              class="btn btn-link"
              @click.stop.prevent="toggleUserRole(user.id)"
            >
              set as user
            </button>
            <button
              v-show="!user.isAdmin"
              type="button"
              class="btn btn-link"
              @click.stop.prevent="toggleUserRole(user.id)"
            >
              set as admin
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import AdminNav from '../components/AdminNav'
const dummyUser = {
  currentUser: {
    id: 1,
    name: 'root',
    email: 'root@example.com',
    image: 'https://i.imgur.com/58ImzMM.png',
    isAdmin: true
  },
  isAuthenticated: true
}
const dummyData = {
  users: [
    {
      id: 1,
      name: 'root',
      email: 'root@example.com',
      password: '$2a$10$OJ3jR93XlEMrQtYMWOIQh.EINWgaRFTXkd0Xi5OC/Vz4maztUXEPe',
      isAdmin: true,
      image: 'https://i.imgur.com/58ImzMM.png',
      createdAt: '2019-07-30T16:24:54.983Z',
      updatedAt: '2019-08-01T10:33:51.095Z',
      isAuthenticated: true
    },
    {
      id: 2,
      name: 'user1',
      email: 'user1@example.com',
      password: '$2a$10$oNyp9cr8jG7NulbUr56g6e3yvwnttFkoBAmtUYAeQuXkcdFz0Ko6y',
      isAdmin: false,
      image: 'https://i.imgur.com/Q14p2le.jpg',
      createdAt: '2019-07-30T16:24:55.204Z',
      updatedAt: '2019-08-01T10:34:15.105Z',
      isAuthenticated: true
    },
    {
      id: 3,
      name: 'user2',
      email: 'user2@example.com',
      password: '$2a$10$tsvcnSFsJvdvs2NLm9rW.uYbah93Xl5cTYcQnSeK3sjEopj.NGzk2',
      isAdmin: false,
      image: 'https://i.imgur.com/OezkRwO.jpg',
      createdAt: '2019-07-30T16:24:55.422Z',
      updatedAt: '2019-08-01T10:34:36.023Z',
      isAuthenticated: true
    }
  ]
}

export default {
  components: {
    AdminNav
  },
  data() {
    return {
      users: []
    }
  },
  created() {
    this.fetchUsers()
  },
  methods: {
    fetchUsers() {
      if (this.users.length === 0) {
        this.users = dummyData.users
        this.users = this.users.map(user => {
          if (user.id !== dummyUser.currentUser.id) return user
          return {
            ...user,
            isCurrentUser: true
          }
        })
      } else {
        return this.users
      }
    },
    toggleUserRole(userId) {
      this.users = this.users.map(user => {
        if (user.id !== userId) return user
        return {
          ...user,
          isAdmin: !user.isAdmin
        }
      })
      this.fetchUsers()
    }
  }
}
</script>

<style scoped>
.btn-link {
  width: 150px;
}
</style>
