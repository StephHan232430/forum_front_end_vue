<template>
  <main role="main">
    <div class="album py-1 bg-light">
      <div class="container">
        <!-- UserProfileCard -->
        <UserProfileCard
          :user="user"
          :is-current-user="currentUser.id === user.id"
          :initial-is-followed="isFollowed"
          :current-user="currentUser"
        />

        <div class="row">
          <div class="col-md-4">
            <!-- UserFollowingsCard -->
            <UserFollowingsCard :followings="Followings" />
            <!-- UserFollowersCard -->
            <UserFollowersCard :followers="Followers" />
          </div>
          <div class="col-md-8">
            <!-- UserCommentsCard -->
            <UserCommentsCard :comments="Comments" />
            <!-- UserFavoritedRestaurantsCard -->
            <UserFavoritedRestaurantsCard :favorited-restaurants="FavoritedRestaurants" />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import UserProfileCard from '../components/UserProfileCard'
import UserFollowingsCard from '../components/UserFollowingsCard'
import UserFollowersCard from '../components/UserFollowersCard'
import UserCommentsCard from '../components/UserCommentsCard'
import UserFavoritedRestaurantsCard from '../components/UserFavoritedRestaurantsCard'
import usersAPI from '../apis/users'
import { Toast } from '../utils/helpers'
import { mapState } from 'vuex'

export default {
  components: {
    UserProfileCard,
    UserFollowingsCard,
    UserFollowersCard,
    UserCommentsCard,
    UserFavoritedRestaurantsCard
  },
  data() {
    return {
      user: {
        id: -1,
        name: '',
        email: '',
        password: '',
        image: '',
        isAdmin: false,
        commentsLength: 0,
        favoritedRestaurantsLength: 0,
        followersLength: 0,
        followingsLength: 0
      },
      isFollowed: false,
      Comments: [],
      FavoritedRestaurants: [],
      Followings: [],
      Followers: []
    }
  },
  computed: {
    ...mapState(['currentUser'])
  },
  created() {
    const { id: userId } = this.$route.params
    this.fetchUser(userId)
  },
  beforeRouteUpdate(to, from, next) {
    const { id } = to.params
    this.fetchUser(id)
    next()
  },
  methods: {
    async fetchUser(userId) {
      try {
        const { data, statusText } = await usersAPI.get({ userId })
        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        this.user = {
          ...this.user,
          id: data.profile.id,
          name: data.profile.name,
          email: data.profile.email,
          password: data.profile.password,
          image: data.profile.image,
          isAdmin: data.profile.isAdmin,
          commentsLength: data.profile.Comments.length,
          favoritedRestaurantsLength: data.profile.FavoritedRestaurants.length,
          followersLength: data.profile.Followers.length,
          followingsLength: data.profile.Followings.length
        }
        this.isFollowed = data.isFollowed
        this.Comments = data.profile.Comments
        this.FavoritedRestaurants = data.profile.FavoritedRestaurants
        this.Followings = data.profile.Followings
        this.Followers = data.profile.Followers
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取得使用者資料，請稍後再試'
        })
      }
    }
  }
}
</script>
