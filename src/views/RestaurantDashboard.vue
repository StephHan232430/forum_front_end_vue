<template>
  <div class="container py-5">
    <div class="row">
      <div class="col-md-12">
        <h1>{{ restaurant.name }}</h1>
        <p>[{{ restaurant.categoryName }}]</p>
        <hr>
        <ul>
          <li>有 {{ restaurant.comments.length }} 筆評論</li>
          <li>有 {{ restaurant.favoritedUsers.length }} 人收藏這家餐廳</li>
        </ul>
        <br />
        <br />

        <a href="javascript:history.back()">回上一頁</a>
      </div>
    </div>
  </div>
</template>

<script>
import restaurantsAPI from '../apis/restaurants'
import { Toast } from '../utils/helpers'
export default {
  data() {
    return {
      restaurant: {
        name: '',
        categoryName: '',
        comments: [],
        favoritedUsers: []
      }
    }
  },
  created() {
    const { id: restaurantId } = this.$route.params
    this.fetchRestaurant(restaurantId)
  },
  methods: {
    async fetchRestaurant(restaurantId) {
      try {
        const { data, statusText } = await restaurantsAPI.getRestaurant({ restaurantId })
        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        this.restaurant = {
          ...this.restaurant,
          name: data.restaurant.name,
          categoryName: data.restaurant.Category.name,
          comments: data.restaurant.Comments,
          favoritedUsers: data.restaurant.FavoritedUsers
        }
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取得餐廳報表，請稍後再試'
        })
      }
    }
  }
}
</script>
