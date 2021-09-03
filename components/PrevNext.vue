<template>
  <div class="flex justify-between">
    <NuxtLink
      v-if="prev"
      :to="{ path: '/chapter/' + getRoutePathFromContentPath(prev.path), params: { slug: prev.slug } }"
      class="float-left"
    >
      <v-icon>mdi-chevron-left</v-icon> {{ prev.description }}
    </NuxtLink>
    <span v-else>&nbsp;</span>
    <NuxtLink
      v-if="next"
      :to="{ path: '/chapter/' + getRoutePathFromContentPath(next.path), params: { slug: next.slug } }"
      class="float-right"
    >
      {{ next.description }} <v-icon>mdi-chevron-right</v-icon>
    </NuxtLink>
    <span v-else>&nbsp;</span>
    <br>
  </div>
</template>

<script>
  export default {
    methods: {
      getRoutePathFromContentPath(path) {
        return (path.indexOf('/') !== -1) ? path.split('/')[2] + '/' + path.split('/')[3] : path
      }
    },
    props: {
      prev: {
        type: Object,
        default: () => null
      },
      next: {
        type: Object,
        default: () => null
      }
    }
  }
</script>