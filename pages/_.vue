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

    try {
      const page = await $content(slug).fetch()

      return { page }
    } catch (e) {
      error({ statusCode: 404, message: 'Page not found' })
    }
  },
  head() {
    return {
      title: this.page.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.page.description,
        },
      ],
      link: [
        {
          rel: 'canonical',
          href: process.env.site.url + this.page.dir,
        },
      ],
    }
  },
}
</script>
