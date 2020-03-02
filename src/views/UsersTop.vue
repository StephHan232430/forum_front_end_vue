<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">
      美食達人
    </h1>
    <hr>
    <div class="row text-center">
      <div
        v-for="user in users"
        :key="user.id"
        class="col-3"
      >
        <router-link :to="{ name: 'user-profile', params: { id: user.id } }">
          <img
            :src="user.image"
            width="140px"
            height="140px"
          >
        </router-link>
        <h2>{{ user.name }}</h2>
        <span class="badge badge-secondary">追蹤人數：{{ user.FollowerCount }}</span>
        <p class="mt-3">
          <button
            v-if="user.isFollowed"
            type="button"
            class="btn btn-danger"
            @click.stop.prevent="deleteFollowship(user.id)"
          >
            取消追蹤
          </button>
          <button
            v-else
            type="button"
            class="btn btn-primary"
            @click.stop.prevent="addFollowship(user.id)"
          >
            追蹤
          </button>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import NavTabs from '../components/NavTabs'
const dummyData = {
  users: [
    {
      id: 1,
      name: 'root',
      email: 'root@example.com',
      password: '$2a$10$J9pLpJJ1Tzfe/ZcjdYwXdumyh.3F5E.w/HTxRcH./cl3azhgekgQe',
      isAdmin: true,
      image: 'https://i.imgur.com/58ImzMM.png',
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      Followers: [
        {
          id: 1,
          name: 'root',
          email: 'root@example.com',
          password: '$2a$10$J9pLpJJ1Tzfe/ZcjdYwXdumyh.3F5E.w/HTxRcH./cl3azhgekgQe',
          isAdmin: true,
          image: null,
          createdAt: '2020-02-28T14:38:32.000Z',
          updatedAt: '2020-02-28T14:38:32.000Z',
          Followship: {
            followerId: 1,
            followingId: 1,
            createdAt: '2020-02-29T08:11:44.000Z',
            updatedAt: '2020-02-29T08:11:44.000Z'
          }
        }
      ],
      FollowerCount: 1,
      isFollowed: true
    },
    {
      id: 2,
      name: 'user1',
      email: 'user1@example.com',
      password: '$2a$10$NyaAtgRuHx3i7hHlnb5IXOC4Uk4.q1J1iQs3op.ymdCEh7.tOwcH2',
      isAdmin: false,
      image: 'https://i.imgur.com/Q14p2le.jpg',
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      Followers: [
        {
          id: 1,
          name: 'root',
          email: 'root@example.com',
          password: '$2a$10$J9pLpJJ1Tzfe/ZcjdYwXdumyh.3F5E.w/HTxRcH./cl3azhgekgQe',
          isAdmin: true,
          image: 'https://i.imgur.com/58ImzMM.png',
          createdAt: '2020-02-28T14:38:32.000Z',
          updatedAt: '2020-02-28T14:38:32.000Z',
          Followship: {
            followerId: 1,
            followingId: 2,
            createdAt: '2020-02-29T08:46:56.000Z',
            updatedAt: '2020-02-29T08:46:56.000Z'
          }
        }
      ],
      FollowerCount: 1,
      isFollowed: true
    },
    {
      id: 3,
      name: 'user2',
      email: 'user2@example.com',
      password: '$2a$10$VHKmtPqbcUzK46qxLllqj.w506U2N2TObMmnpdlNG2CLZPa1xzuTi',
      isAdmin: false,
      image: 'https://i.imgur.com/OezkRwO.jpg',
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      Followers: [],
      FollowerCount: 0,
      isFollowed: false
    }
  ]
}
export default {
  components: {
    NavTabs
  },
  data() {
    return {
      users: []
    }
  },
  created() {
    return this.fetchUsers()
  },
  methods: {
    fetchUsers() {
      this.users = dummyData.users
    },
    addFollowship(id) {
      const targetUser = this.users.find(user => user.id === id)
      targetUser.isFollowed = true
      targetUser.FollowerCount += 1
    },
    deleteFollowship(id) {
      const targetUser = this.users.find(user => user.id === id)
      targetUser.isFollowed = false
      targetUser.FollowerCount -= 1
    }
  }
}
</script>
