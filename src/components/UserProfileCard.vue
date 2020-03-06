<template>
  <div class="card my-5">
    <div class="row no-gutters">
      <div class="col-md-4 text-center">
        <img
          :src="user.image"
          width="300px"
          height="300px"
        >
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">
            {{ user.name }}
          </h5>
          <p class="card-text">
            {{ user.email }}
          </p>
          <ul class="list-unstyled list-inline">
            <li><strong>{{ user.commentsLength }}</strong> 已評論餐廳</li>
            <li><strong>{{ user.favoritedRestaurantsLength }}</strong> 收藏的餐廳</li>
            <li><strong>{{ user.followingsLength }}</strong> followings (追蹤者)</li>
            <li><strong>{{ user.followersLength }}</strong> followers (追隨者)</li>
          </ul>
          <p v-if="isCurrentUser">
            <router-link
              :to="{ name: 'user-profile-edit', params: { id: user.id } }"
              role="button"
              class="btn btn-primary"
            >
              edit
            </router-link>
          </p>
          <p v-else>
            <button
              v-if="isFollowed"
              type="button"
              class="btn btn-danger"
              @click.stop.prevent="deleteFollowing(user.id)"
            >
              取消追蹤
            </button>
            <button
              v-else
              type="button"
              class="btn btn-primary"
              @click.stop.prevent="addFollowing(user.id)"
            >
              追蹤
            </button>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import usersAPI from '../apis/users'
import { Toast } from '../utils/helpers'
export default {
  props: {
    user: {
      type: Object,
      required: true
    },
    isCurrentUser: {
      type: Boolean,
      required: true
    },
    initialIsFollowed: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      isFollowed: this.initialIsFollowed
    }
  },
  watch: {
    initialIsFollowed(isFollowed) {
      this.isFollowed = isFollowed
    }
  },
  methods: {
    async addFollowing(userId) {
      try {
        const { data, statusText } = await usersAPI.addFollowing({ userId })
        if (statusText !== 'OK' || data.status !== 'success') {
          throw new Error(statusText)
        }
        this.isFollowed = true
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法追蹤使用者，請稍後再試'
        })
      }
    },
    async deleteFollowing(userId) {
      try {
        const { data, statusText } = await usersAPI.deleteFollowing({ userId })
        if (statusText !== 'OK' || data.status !== 'success') {
          throw new Error(statusText)
        }
        this.isFollowed = false
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取消追蹤使用者，請稍後再試'
        })
      }
    }
  }
}
</script>
