<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">
      人氣餐廳
    </h1>

    <hr>
    <div
      v-for="restaurant in restaurants"
      :key="restaurant.id"
      class="card mb-3"
      style="max-width: 540px;margin: auto;"
    >
      <div class="row no-gutters">
        <div class="col-md-4">
          <router-link :to="{ name: 'restaurant', params: { id: restaurant.id } }">
            <img
              class="card-img"
              :src="restaurant.image"
            >
          </router-link>
        </div>
        <div class="col-md-8">
          <div class="card-body">
            <h5 class="card-title">
              {{ restaurant.name }}
            </h5>
            <span class="badge badge-secondary">收藏數：{{ restaurant.FavoriteCount }}</span>
            <p class="card-text">
              {{ restaurant.description }}
            </p>
            <router-link
              :to="{ name: 'restaurant', params: { id: restaurant.id } }"
              class="btn btn-primary mr-2"
            >
              Show
            </router-link>

            <button
              v-if="restaurant.isFavorited"
              type="button"
              class="btn btn-danger mr-2"
              @click.stop.prevent="removeFavorite(restaurant.id)"
            >
              移除最愛
            </button>
            <button
              v-else
              type="button"
              class="btn btn-primary"
              @click.stop.prevent="addFavorite(restaurant.id)"
            >
              加到最愛
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavTabs from '../components/NavTabs'
const dummyUser = {
  currentUser: {
    id: 1,
    name: '管理者',
    email: 'root@example.com',
    image: 'https://i.pravatar.cc/300',
    isAdmin: true
  },
  isAuthenticated: true
}
const dummyData = {
  restaurants: [
    {
      id: 3,
      name: 'Herminio Nicolas',
      tel: '063-480-5811 x2308',
      address: '52267 Marquardt Corners',
      opening_hours: '08:00',
      description: 'autem',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=38.32006179376239',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 1,
      FavoritedUsers: [
        {
          id: 1,
          name: 'root',
          email: 'root@example.com',
          password: '$2a$10$J9pLpJJ1Tzfe/ZcjdYwXdumyh.3F5E.w/HTxRcH./cl3azhgekgQe',
          isAdmin: true,
          image: null,
          createdAt: '2020-02-28T14:38:32.000Z',
          updatedAt: '2020-02-28T14:38:32.000Z',
          Favorite: {
            UserId: 1,
            RestaurantId: 3,
            createdAt: '2020-02-29T16:54:54.000Z',
            updatedAt: '2020-02-29T16:54:54.000Z'
          }
        }
      ],
      isFavorited: true,
      FavoriteCount: 1
    },
    {
      id: 1042,
      name: '恬甜紅豆餅',
      tel: '0921 724 745',
      address: '241新北市三重區中正北路145巷2號',
      opening_hours: '14:00',
      description: '用料新鮮，價格平民，東西超好吃，尤其布丁超香，紅豆不甜很香，芋頭很香。',
      image: 'https://i.imgur.com/1YlJCEi.jpg',
      viewCounts: 0,
      createdAt: '2020-02-29T11:47:37.000Z',
      updatedAt: '2020-03-01T02:43:18.000Z',
      CategoryId: 392,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 1012,
      name: 'AC 壽司',
      tel: '00000000',
      address: 'aaaaaaaaa',
      opening_hours: '00:59',
      description: '',
      image: null,
      viewCounts: 0,
      createdAt: '2020-02-29T04:13:20.000Z',
      updatedAt: '2020-02-29T14:29:43.000Z',
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 48,
      name: 'Fernando Beahan',
      tel: '782-526-8659 x452',
      address: '447 Isaiah Garden',
      opening_hours: '08:00',
      description: 'officia corrupti nisi',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=49.159643827730235',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 1,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 47,
      name: 'Flo Paucek',
      tel: '616-595-6381',
      address: '0570 Toy Land',
      opening_hours: '08:00',
      description: 'Explicabo voluptate quam.',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=7.110473595831435',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 4,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 46,
      name: 'Mr. Price Heathcote',
      tel: '1-234-170-6861',
      address: '178 Collins Mission',
      opening_hours: '08:00',
      description: 'Unde maiores error ex.',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=38.75335489939356',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 4,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 45,
      name: 'Anjali Kassulke',
      tel: '313-500-9523 x7668',
      address: '356 Reanna Fall',
      opening_hours: '08:00',
      description: 'dolore',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=37.65242985288428',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 1,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 44,
      name: 'Madge Moen',
      tel: '794.584.3958 x1725',
      address: '6233 Annamae Mission',
      opening_hours: '08:00',
      description: 'sed ut illum',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=33.163077027772346',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 43,
      name: 'Mrs. Noemy Watsica',
      tel: '(252) 981-9430 x3227',
      address: '1218 Dariana Ports',
      opening_hours: '08:00',
      description: 'Animi rerum suscipit iusto id autem et. Facilis id',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=23.889387774329695',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 5,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 42,
      name: 'Adrian Weimann',
      tel: '(102) 131-9224 x2147',
      address: '04294 Nils Track',
      opening_hours: '08:00',
      description: 'Et dolores eos sed aut doloribus velit autem quae.',
      image: 'https://loremflickr.com/320/240/restaurant,food/?random=57.71296646754862',
      viewCounts: 0,
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z',
      CategoryId: 4,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    }
  ]
}
export default {
  components: {
    NavTabs
  },
  data() {
    return {
      currentUser: dummyUser.currentUser,
      restaurants: dummyData.restaurants
    }
  },
  methods: {
    addFavorite(restaurantId) {
      const targetRestaurant = this.restaurants.find(restaurant => restaurant.id === restaurantId)
      targetRestaurant.isFavorited = true
      targetRestaurant.FavoriteCount += 1
      this.restaurants = this.restaurants.sort((a, b) => b.FavoriteCount - a.FavoriteCount)
    },
    removeFavorite(restaurantId) {
      const targetRestaurant = this.restaurants.find(restaurant => restaurant.id === restaurantId)
      targetRestaurant.isFavorited = false
      targetRestaurant.FavoriteCount -= 1
      this.restaurants = this.restaurants.sort((a, b) => b.FavoriteCount - a.FavoriteCount)
    }
  }
}
</script>
