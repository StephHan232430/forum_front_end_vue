<template>
  <div class="card mb-3">
    <div class="card-header">
      <strong>{{ followings.length }}</strong> followings (追蹤者)
    </div>
    <div class="card-body">
      <router-link
        v-for="following in followings"
        :key="following.id"
        :to="{ name: 'user-profile', params: { id: following.id } }"
      >
        <img
          :src="following.image"
          width="60"
          height="60"
          class="avatar"
        >
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    followings: {
      type: Array,
      required: true
    }
  }
}
</script>
