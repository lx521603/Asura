<script setup lang="ts">
import type { ContentNavigationItem } from '@nuxt/content'

const route = useRoute()

const navigation = inject<Ref<ContentNavigationItem[]>>('navigation')

const { open: searchOpen } = useContentSearch()

const open = ref(false)

const isDocs = computed(() => route.path === '/docs' || route.path.startsWith('/docs/'))

// Both modals portal to `body` with no z-index, so after a client-side layout
// change the menu can end up painted over the search
watch(searchOpen, (value) => {
  if (value) {
    open.value = false
  }
})

const items = computed(() => [{
  label: '首页',
  to: '/',
  active: isDocs.value
},{
  label: '折腾笔记',
  to: '/docs',
  active: isDocs.value
}, {
  label: 'Blog',
  to: '/blog'
}, {
  label: '动态',
  to: '/changelog'
}])
</script>

<template>
  <UHeader v-model:open="open">
    <template #left>
      <NuxtLink
        to="/"
        class="focus-visible:outline-3 outline-primary/25 rounded-md p-1 -ms-1"
      >
        <AppLogo class="w-auto h-6 shrink-0" />
      </NuxtLink>

      <!-- <TemplateMenu /> -->
    </template>

    <UNavigationMenu
      :items="items"
      variant="link"
    />

    <template #right>
      <UColorModeButton />

      <UContentSearchButton class="lg:hidden" />

      <!-- 桌面端与移动端顶部右侧：纯图标跳转 -->
      <UButton
        icon="i-simple-icons-x"
        color="neutral"
        variant="ghost"
        to="https://go.91.pt/x"
        target="_blank"
        class="lg:hidden"
      />

      <UButton
        icon="i-simple-icons-x"
        color="neutral"
        variant="ghost"
        to="https://go.91.pt/x"
        target="_blank"
        class="hidden lg:inline-flex"
      />

      <UButton
        icon="i-simple-icons-instagram"
        color="neutral"
        variant="ghost"
        to="https://go.91.pt/instagram"
        target="_blank"
        class="hidden lg:inline-flex"
      />
    </template>

    <template #body>
      <UNavigationMenu
        :items="items"
        orientation="vertical"
        class="-mx-2.5"
      />

      <template v-if="isDocs">
        <USeparator class="my-6" />

        <UContentNavigation
          :navigation="navigation"
          highlight
        />
      </template>

      <USeparator class="my-6" />

      <!-- 移动端侧边栏底部：改成水平排列的纯图标，或者保留整行图标按钮 -->
      <div class="flex items-center gap-2">
        <UButton
          icon="i-simple-icons-x"
          color="neutral"
          variant="subtle"
          to="https://go.91.pt/x"
          target="_blank"
          block
          class="flex-1"
        />
        <UButton
          icon="i-simple-icons-instagram"
          color="neutral"
          variant="subtle"
          to="https://go.91.pt/instagram"
          target="_blank"
          block
          class="flex-1"
        />
      </div>

      <!-- 
      <UButton
        label="Sign in"
        color="neutral"
        variant="subtle"
        to="/login"
        block
        class="mb-3"
      />
      <UButton
        label="Sign up"
        color="neutral"
        to="/signup"
        block
      /> -->
    </template>
  </UHeader>
</template>