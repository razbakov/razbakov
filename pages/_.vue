<template>
  <div class="prose prose-sm sm:prose lg:prose-lg xl:prose-2xl mx-auto">
    <h1 v-if="page.title">{{ page.title }}</h1>
    <NuxtContent :document="page" />
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    const slug = params.pathMatch || 'index'

    let page = {}

    try {
      page = await $content(slug).fetch()
    } catch (e) {
      error({ statusCode: 404, message: 'Page not found' })
    }

    return {
      page,
    }
  },
}
</script>
