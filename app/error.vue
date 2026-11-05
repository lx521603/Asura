<script setup lang="ts">
import type { NuxtError } from '#app'

defineProps<{
  error: NuxtError
}>()

useHead({
  htmlAttrs: {
    lang: 'zh_CN'
  }
})

useSeoMeta({
  title: '页面没有找到',
  description: '对不起，页面不存在'
})

const { data: navigation } = await useAsyncData('navigation', () => queryCollectionNavigation('docs'), {
  transform: data => data.find(item => item.path === '/docs')?.children || []
})
const { data: files } = useLazyAsyncData('search', () => queryCollectionSearchSections('docs'), {
  server: false
})

provide('navigation', navigation)

const handleError = () => clearError({ redirect: '/' })
</script>

<template>
  <UApp>
    <AppHeader />

    <UMain>
      <UContainer>
        <div class="min-h-[85vh] flex flex-col items-center justify-center text-center py-20">
          
        
          <!-- 错误状态码 -->
          <span class="text-6xl font-extrabold text-primary mb-4">{{ error.statusCode }}</span>
          
          <!-- 错误提示文本 -->
          <h1 class="text-2xl font-bold mb-2">页面没有找到</h1>
          <p class="text-muted mb-8">对不起，您访问的页面不存在或已被移动。</p>

          <!-- 按钮区域 -->
          <div class="flex gap-4">
            <UButton
              label="返回首页"
              icon="i-heroicons-home"
              size="lg"
              @click="handleError"
            />
            <UButton
              label="访问 X 官方"
              icon="i-simple-icons-x"
              color="neutral"
              variant="outline"
              size="lg"
              to="https://go.91.pt/x"
              target="_blank"
            />
          </div>

        </div>
      </UContainer>
    </UMain>

    <AppFooter />

    <ClientOnly>
      <LazyUContentSearch
        :files="files"
        :navigation="navigation"
        :fuse="{ resultLimit: 42 }"
      />
    </ClientOnly>
  </UApp>
</template>