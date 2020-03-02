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
              :to="{ name: 'user-profile-edit', params: { id: currentUser.id } }"
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
              @click.stop.prevent="deleteFollowing()"
            >
              取消追蹤
            </button>
            <button
              v-else
              type="button"
              class="btn btn-primary"
              @click.stop.prevent="addFollowing()"
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
    },
    currentUser: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isFollowed: this.initialIsFollowed
    }
  },
  methods: {
    addFollowing() {
      this.isFollowed = true
    },
    deleteFollowing() {
      this.isFollowed = false
    }
  }
}
</script>
