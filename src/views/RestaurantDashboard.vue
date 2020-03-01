<template>
  <div class="container py-5">
    <div class="row">
      <div class="col-md-12">
        <h1>{{ restaurant.name }}</h1>
        <p>[{{ restaurant.Category.name }}]</p>
        <hr>
        <ul>
          <li>有 {{ restaurant.Comments.length }} 筆評論</li>
          <li>有 {{ restaurant.FavoritedUsers.length }} 人收藏這家餐廳</li>
        </ul>
        <br />
        <br />

        <a href="javascript:history.back()">回上一頁</a>
      </div>
    </div>
  </div>
</template>

<script>
const dummyData = {
  restaurant: {
    id: 1,
    name: 'Dr. Izaiah Ernser',
    tel: '389.061.3241 x162',
    address: '7226 Johns Islands',
    opening_hours: '08:00',
    description: 'quam',
    image: 'https://loremflickr.com/320/240/restaurant,food/?random=90.45122643685453',
    viewCounts: 2,
    createdAt: '2020-02-28T14:38:32.000Z',
    updatedAt: '2020-03-01T03:00:40.000Z',
    CategoryId: 2,
    Category: {
      id: 2,
      name: '日本料理',
      createdAt: '2020-02-28T14:38:32.000Z',
      updatedAt: '2020-02-28T14:38:32.000Z'
    },
    Comments: [
      {
        id: 1,
        text: 'Dolores dolorem maxime debitis blanditiis quam sed voluptatem mollitia.',
        UserId: 3,
        RestaurantId: 1,
        createdAt: '2020-02-28T14:38:32.000Z',
        updatedAt: '2020-02-28T14:38:32.000Z',
        User: {
          id: 3,
          name: 'user2',
          email: 'user2@example.com',
          password: '$2a$10$VHKmtPqbcUzK46qxLllqj.w506U2N2TObMmnpdlNG2CLZPa1xzuTi',
          isAdmin: false,
          image: null,
          createdAt: '2020-02-28T14:38:32.000Z',
          updatedAt: '2020-02-28T14:38:32.000Z'
        }
      },
      {
        id: 51,
        text: 'Nobis aspernatur nobis cumque sed nemo et autem.',
        UserId: 2,
        RestaurantId: 1,
        createdAt: '2020-02-28T14:38:32.000Z',
        updatedAt: '2020-02-28T14:38:32.000Z',
        User: {
          id: 2,
          name: 'user1',
          email: 'user1@example.com',
          password: '$2a$10$NyaAtgRuHx3i7hHlnb5IXOC4Uk4.q1J1iQs3op.ymdCEh7.tOwcH2',
          isAdmin: false,
          image: null,
          createdAt: '2020-02-28T14:38:32.000Z',
          updatedAt: '2020-02-28T14:38:32.000Z'
        }
      },
      {
        id: 101,
        text: 'Non nesciunt eveniet pariatur sit id officia consequuntur.',
        UserId: 1,
        RestaurantId: 1,
        createdAt: '2020-02-28T14:38:32.000Z',
        updatedAt: '2020-02-28T14:38:32.000Z',
        User: {
          id: 1,
          name: 'root',
          email: 'root@example.com',
          password: '$2a$10$J9pLpJJ1Tzfe/ZcjdYwXdumyh.3F5E.w/HTxRcH./cl3azhgekgQe',
          isAdmin: true,
          image: null,
          createdAt: '2020-02-28T14:38:32.000Z',
          updatedAt: '2020-02-28T14:38:32.000Z'
        }
      }
    ],
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
    ]
  }
}
export default {
  data() {
    return {
      restaurant: dummyData.restaurant
    }
  }
}
</script>
