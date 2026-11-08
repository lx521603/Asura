<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryCollection('landing').path('/').first())
if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

const title = page.value.seo?.title || page.value.title
const description = page.value.seo?.description || page.value.description

useSeoMeta({
  titleTemplate: '',
  title,
  ogTitle: title,
  description,
  ogDescription: description,
  ogImage: 'https://ui.nuxt.com/assets/templates/nuxt/saas-light.png'
})
</script>

<template>
  <div v-if="page">
    <!-- 如果你想在页面最上方保留 ContentRenderer 渲染的 Markdown 内容，可以放这里 -->
    <div class="w-full -mx-0 sm:-mx-0 lg:-mx-8">
      <ContentRenderer
        :value="page"
        :prose="false"
        :components="{ a: 'prose-a' }"
      />
    </div>

    
  </div>
</template>