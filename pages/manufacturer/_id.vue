<template>
  <div class="column">
    <h1 class="title is-3">
      {{ manufacturer.name }}
      &nbsp;
      <a :class="['title-link']" :href="manufacturer.url" target="_blank">
        <span class="icon is-small">
          <font-awesome-icon :icon="['fas', 'link']" />
        </span>
      </a>
    </h1>
    <figure class="image is-128x128">
      <img :src="manufacturer.image">
    </figure>
    <hr>
    <h2 class="subtitle">
      Sortiment
    </h2>
    <div class="columns is-multiline is-centered">
      <div v-for="(feed) in manufacturer.feeds" :key="feed.feed_id" class="column is-one-third">
        <div class="card">
          <div class="card-content">
            <p class="title">
              {{ feed.name }}
            </p>
            <p class="subtitle">
              ...
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ERROR_TYPES } from '~/const'

export default {
  data () {
    return {
      id: this.$route.params.id,
      manufacturer: []
    }
  },
  mounted () {
    this.getManufacturer()
  },
  methods: {
    getManufacturer () {
      this.$axios.$get('http://localhost/api/manufacturers/' + this.id)
        .then((response) => {
          this.manufacturer = response
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
      title: 'Hersteller'
    }
  }
}
</script>
