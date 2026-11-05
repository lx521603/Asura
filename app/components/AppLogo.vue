<script setup lang="ts">
const props = defineProps<{
  white?: boolean
}>()

const color = computed(() => props.white ? 'text-white' : 'text-black dark:text-white')

const isHovering = ref(false)
const isBlinking = ref(false)
const isTouchDevice = ref(false)

let blinkTimer: ReturnType<typeof setInterval> | undefined

onMounted(() => {
  isTouchDevice.value = 'ontouchstart' in window || navigator.maxTouchPoints > 0
  blinkTimer = setInterval(() => {
    isBlinking.value = true
    setTimeout(() => { isBlinking.value = false }, 180)
  }, 5000)
})

onBeforeUnmount(() => {
  if (blinkTimer) clearInterval(blinkTimer)
})
</script>

<template>
  <span class="flex items-center gap-2" :class="color">
    <svg
      viewBox="0 0 32 32"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      class="size-6"
      @mouseenter="isHovering = true"
      @mouseleave="isHovering = false"
    >
      <path
        v-if="isBlinking || (isHovering && !isTouchDevice)"
        fill-rule="evenodd"
        clip-rule="evenodd"
        d="M16.0752 0C7.21095 0 0 7.18587 0 16.0501V30.3853C0 31.2771 0.722918 32 1.61468 32H16.1002C24.9367 32 32.1002 24.8366 32.1002 16C32.1002 7.16344 24.9117 0 16.0752 0ZM24.802 16.0501C26.697 16.0501 28.2332 17.8658 28.2332 15.9708C28.2332 15.9708 26.697 15 24.802 15C22.907 15 21.3708 15.9708 21.3708 15.9708C21.3708 17.8658 22.907 16.0501 24.802 16.0501ZM14.6182 16.0501C16.5133 16.0501 18.0494 17.866 18.0494 15.971C18.0494 15.971 16.395 15 14.5 15C12.605 15 11.1871 15.971 11.1871 15.971C11.1871 17.866 12.7233 16.0501 14.6182 16.0501Z"
        fill="currentColor"
      />
      <path
        v-else
        fill-rule="evenodd"
        clip-rule="evenodd"
        d="M16.0752 0C7.21095 0 0 7.18587 0 16.0501V30.3853C0 31.2771 0.722918 32 1.61468 32H16.1002C24.9367 32 32.1002 24.8366 32.1002 16C32.1002 7.16344 24.9117 0 16.0752 0ZM24.802 19.402C26.697 19.402 28.2332 17.8658 28.2332 15.9708C28.2332 14.0758 26.697 12.5396 24.802 12.5396C22.907 12.5396 21.3708 14.0758 21.3708 15.9708C21.3708 17.8658 22.907 19.402 24.802 19.402ZM14.6182 19.4022C16.5133 19.4022 18.0494 17.866 18.0494 15.971C18.0494 14.076 16.5133 12.5398 14.6182 12.5398C12.7233 12.5398 11.1871 14.076 11.1871 15.971C11.1871 17.866 12.7233 19.4022 14.6182 19.4022Z"
        fill="currentColor"
      />
    </svg>
    <span class="font-bold text-2xl">Asura</span>
  </span>
</template>