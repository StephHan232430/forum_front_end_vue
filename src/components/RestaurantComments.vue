<template>
  <div>
    <h2 class="my-4">
      所有評論：
    </h2>

    <div>
      <blockquote
        v-for="comment in comments"
        :key="comment.id"
        class="blockquote mb-0"
      >
        <button
          v-if="currentUser.isAdmin"
          type="button"
          class="btn btn-danger float-right"
          @click.stop.prevent="handleDeleteButtonClick(comment.id)"
        >
          Delete
        </button>
        <h3>
          <router-link :to="{ name: 'user-profile', params: { id: comment.User.id } }">
            {{ comment.User.name }}
          </router-link>
        </h3>
        <p>{{ comment.text }}</p>
        <footer class="blockquote-footer">
          {{ comment.createdAt | fromNow }}
        </footer>
      </blockquote>
      <hr>
    </div>
  </div>
</template>

<script>
import { fromNowFilter } from '../utils/mixins'
import restaurantsAPI from '../apis/restaurants'
import { Toast } from '../utils/helpers'
import { mapState } from 'vuex'

export default {
  mixins: [fromNowFilter],
  props: {
    restaurantComments: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      comments: this.restaurantComments
    }
  },
  computed: {
    ...mapState(['currentUser'])
  },
  watch: {
    restaurantComments(comments) {
      this.comments = {
        ...this.comments,
        ...comments
      }
    }
  },
  methods: {
    handleDeleteButtonClick(commentId) {
      console.log('handleDeleteButtonClick', commentId)

      this.$emit('after-delete-comment', commentId)
    }
  }
}
</script>
