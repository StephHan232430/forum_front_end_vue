<template>
  <Spinner v-if="isLoading" />
  <table
    v-else
    class="table"
  >
    <thead class="thead-dark">
      <tr>
        <th scope="col">
          #
        </th>
        <th scope="col">
          Category
        </th>
        <th scope="col">
          Name
        </th>
        <th
          scope="col"
          width="300"
        >
          操作
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="restaurant in restaurants"
        :key="restaurant.id"
      >
        <th scope="row">
          {{ restaurant.id }}
        </th>
        <td>{{ restaurant.Category && restaurant.Category.name }}</td>
        <td>{{ restaurant.name }}</td>
        <td class="d-flex justify-content-between">
          <router-link
            :to="{ name: 'admin-restaurant', params: { id: restaurant.id } }"
            class="btn btn-link"
          >
            Show
          </router-link>

          <router-link
            :to="{ name: 'admin-restaurant-edit', params: { id: restaurant.id } }"
            class="btn btn-link"
          >
            Edit
          </router-link>

          <button
            type="button"
            class="btn btn-link"
            @click.stop.prevent="deleteRestaurant(restaurant.id)"
          >
            Delete
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import adminAPI from '../apis/admin'
import { Toast } from '../utils/helpers'
import Spinner from './Spinner'

export default {
  components: {
    Spinner
  },
  data() {
    return {
      restaurants: [],
      isLoading: true
    }
  },
  created() {
    this.fetchRestaurants()
  },
  methods: {
    async fetchRestaurants() {
      try {
        const { data, statusText } = await adminAPI.restaurants.get()
        if (statusText !== 'OK') {
          throw new Error(statusText)
        }
        this.restaurants = data.restaurants
        this.isLoading = false
      } catch (error) {
        this.isLoading = false
        Toast.fire({
          icon: 'error',
          title: '無法取得餐廳清單，請稍後再試'
        })
      }
    },
    async deleteRestaurant(restaurantId) {
      try {
        // 透過API，"實際"刪除back-end資料
        const { data, statusText } = await adminAPI.restaurants.delete({ restaurantId })
        if (statusText !== 'OK' || data.status !== 'success') {
          throw new Error(statusText)
        }
        // 整理front-end資料，使"頁面呈現"與back-end同步
        this.restaurants = this.restaurants.filter(restaurant => restaurant.id !== restaurantId)
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法刪除餐廳，請稍後再試',
          type: 'error'
        })
      }
    }
  }
}
</script>
