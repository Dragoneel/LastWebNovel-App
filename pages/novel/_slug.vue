<template>
  <v-card>
    <v-card-title class="headline">
      <a @click="$router.go(-1)">
        <v-icon>mdi-chevron-left</v-icon>
      </a>
      {{ summary.title }}
    </v-card-title>
    <v-card-subtitle>
      {{ summary.description }} <br>
      <b>Status:</b> {{ summary.status }} <br>
      <b>Author:</b> {{ summary.author }} <br>
      <b>Genres:</b> {{ summary.genres }} <br>
      <b>Theme:</b> {{ summary.themes }}  <br>
    </v-card-subtitle>
    <v-card-text>
      <nuxt-content :document="summary" />
    </v-card-text>
    <!-- List of chapters -->
    <v-list>
      <v-list-item
        v-for="(item, i) in chapters"
        :key="i"
        router
        exact
      >
        <v-list-item-content>
          <nuxt-link :to="{ path: '/chapter/' + getSlugFromSummary(summary.slug) + '/' + item.slug, params: { slug: item.slug, folder: summary.slug } }">
            <v-list-item-title v-text="item.title" />
            <v-list-item-subtitle v-text="item.description" />
          </nuxt-link>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script>
export default {
  methods: {
    getSlugFromSummary(txt) {
      return (txt.indexOf('_') !== -1) ? txt.split('_')[1] : txt
    }
  },
  async asyncData ({ $content, params }) {
    let novelSlug = params.slug
    const summary = await $content('novels/summary_' + novelSlug).fetch()

    let chapters = {}
    try {
      chapters = await $content('novels/'+ novelSlug)
        .sortBy('title', 'asc')
        .fetch()
    }
    catch (error) {
      console.warn("No chapters available");
    }

    return {
      summary,
      chapters
    }
  }
}
</script>

<style>

</style>