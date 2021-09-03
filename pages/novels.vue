<template>
<div>
  <div
    v-for="(item, i) in novels"
    :key="i"
    router
    exact  
  >
    <novel-card
      color="blue"
      full-header
      :status="item.status"
      :author="item.author"
      :genres="item.genres"
    >
      <template #heading>
        <nuxt-link :to="{ path: 'novel/'+ getSlugFromSummary(item.slug), params: { slug: item.slug } }">
          <div class="pa-3 white--text">
            <div class="text-h5 font-weight-light">
              {{item.title}}
            </div>
            <div class="text-caption">
              {{item.description}}
            </div>
          </div>
        </nuxt-link>
      </template>
    </novel-card>
  </div>
  </div>
  <!-- <v-card>
    <v-card-title class="headline">
      Web Novels
    </v-card-title>
    <v-card-subtitle>
      Sponsored novels
    </v-card-subtitle>
    <v-card-text>
      <v-list>
        <v-list-item
          v-for="(item, i) in novels"
          :key="i"
          router
          exact
        >
          <v-list-item-content>
            <nuxt-link :to="{ path: 'novel/'+ getSlugFromSummary(item.slug), params: { slug: item.slug } }">
              <v-list-item-title v-text="item.title" />
              <v-list-item-subtitle v-text="item.description" />
            </nuxt-link>
            <b>Status:</b> <v-list-item-subtitle v-text="item.status" />
            <b>Author:</b> <v-list-item-subtitle v-text="item.author" />
            <b>Genres:</b> <v-list-item-subtitle v-text="item.genres" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-card-text>
  </v-card> -->
</template>

<script>
export default {
  methods: {
    getSlugFromSummary(txt) {
      return txt.split('_')[1]
    }
  },
  async asyncData ({ $content }) {
    const novels = await $content('novels')
      .sortBy('title', 'asc')
      .where({
        'slug': {
          $regex: ['summary', 'i']
        }
      })
      .without('body')
      .fetch()

    return {
      novels
    }
  }
}
</script>
