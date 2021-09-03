<template>
  <v-card>
    <v-card-title class="headline">
      <a @click="$router.go(-1)">
        <v-icon>mdi-chevron-left</v-icon>
      </a> 
      {{ chapter.title }}
    </v-card-title>
    <v-card-subtitle>
      {{ chapter.description }} <br>
      <prev-next :prev="prev" :next="next" />
    </v-card-subtitle>
    <v-card-text>
      <nuxt-content :document="chapter" />
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  async asyncData ({ $content, params }) {
    let chapterFolder = params.novel
    let chapterSlug = params.slug
    let chapter = {}

    try {
      chapter = await $content('novels/'+ chapterFolder + '/' + chapterSlug).fetch()
    }
    catch (error) {
      console.warn("Chapter not available");
    }
    
    let [prev, next] = await $content('novels/'+ chapterFolder)
      .only(['title', 'slug', 'description'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

    return {
      chapter,
      prev,
      next
    }
  }
}   
</script>

<style>

</style>