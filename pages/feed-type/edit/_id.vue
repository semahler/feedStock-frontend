<template>
  <div class="column">
    <h1 class="title is-3">
      Futterart bearbeiten
    </h1>
    <hr>
    <feed-type-new-edit-form-component :feed-type="feedType" />
  </div>
</template>

<script>
import FeedTypeNewEditFormComponent from '@/components/forms/FeedTypeNewEditFormComponent'
import { ERROR_TYPES } from '~/const'

export default {
  components: {
    FeedTypeNewEditFormComponent
  },
  data () {
    return {
      id: this.$route.params.id,
      feedType: {
        id: null,
        title: null
      }
    }
  },
  mounted () {
    this.getFeedType()
  },
  methods: {
    getFeedType () {
      this.$axios.$get('http://127.0.0.1/api/feed_types/' + this.id)
        .then((response) => {
          this.feedType = response
        })
        .catch((error) => {
          if (error.response) {
            this.showErrorNotification(ERROR_TYPES.REQUEST_ERROR, error.response)
          } else if (error.request) {
            this.showErrorNotification(ERROR_TYPES.CONNECTION_ERROR)
          } else {
            this.showErrorNotification(ERROR_TYPES.GENERAL_ERROR, error.message)
          }
        })
    },
    showErrorNotification (errorType, errorDetails) {
      this.$store.commit('modules/notifications/setErrorNotificationData', { errorType, errorDetails })
    }
  },
  head () {
    return {
      title: 'Futterart bearbeiten'
    }
  }
}
</script>
