<template>
  <div class="container py-5">
    <NavTabs />
    <!-- 餐廳類別標籤 RestaurantsNavPills -->
    <RestaurantsNavPills :categories="categories" />
    <Spinner v-if="isLoading" />
    <template>
      <div class="row">
        <!-- 餐廳卡片 RestaurantCard-->
        <RestaurantCard
          v-for="restaurant in restaurants"
          :key="restaurant.id"
          :initial-restaurant="restaurant"
        />
      </div>

      <!-- 分頁標籤 RestaurantPagination -->
      <RestaurantsPagination
        v-if="totalPage > 1"
        :total-page="totalPage"
        :current-page="currentPage"
        :category-id="categoryId"
      />

      <div v-if="restaurants.length < 1">
        此類別目前無餐廳資料
      </div>
    </template>
  </div>
</template>

<script>
import NavTabs from './../components/NavTabs'
import RestaurantCard from './../components/RestaurantCard'
import RestaurantsNavPills from './../components/RestaurantsNavPills'
import RestaurantsPagination from './../components/RestaurantsPagination'
import restaurantsAPI from '../apis/restaurants'
import { Toast } from '../utils/helpers'
import Spinner from '../components/Spinner'

export default {
  components: {
    NavTabs,
    RestaurantCard,
    RestaurantsNavPills,
    RestaurantsPagination,
    Spinner
  },
  data() {
    return {
      restaurants: [],
      categories: [],
      categoryId: -1,
      currentPage: 1,
      totalPage: -1,
      isLoading: true
    }
  },
  created() {
    const { page, categoryId } = this.$route.query
    this.fetchRestaurants({ page, categoryId })
  },
  beforeRouteUpdate(to, from, next) {
    const { page, categoryId } = to.query
    this.fetchRestaurants({ page, categoryId })
    next()
  },
  methods: {
    async fetchRestaurants({ page = 1, categoryId = '' }) {
      try {
        const { data, statusText } = await restaurantsAPI.getRestaurants({
          page,
          categoryId
        })
        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        this.categories = data.categories
        this.categoryId = data.categoryId
        this.currentPage = data.page
        this.restaurants = data.restaurants
        this.totalPage = data.totalPage.length
        this.isLoading = false
      } catch (error) {
        this.isLoading = false
        Toast.fire({
          icon: 'error',
          title: '無法取得餐廳資料，請稍後再試'
        })
      }
    }
  }
}
</script>
