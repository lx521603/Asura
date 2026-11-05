<script setup lang="ts">


const toast = useToast()

const email = ref('')
const loading = ref(false)


</script>

<template>
  <USeparator
    icon="i-simple-icons-nuxtdotjs"
    class="h-px"
  />

  <UFooter :ui="{ top: 'border-b border-default' }">


    <template #left>
      <p class="text-muted text-sm">
        Asura.im • © {{ new Date().getFullYear() }}
      </p>
    </template>

    <template #right>
      <UButton
        to="https://go.91.pt/discord"
        target="_blank"
        icon="i-simple-icons-discord"
        aria-label="Asura on Discord"
        color="neutral"
        variant="ghost"
      />
      <UButton
        to="https://go.91.pt/x"
        target="_blank"
        icon="i-simple-icons-x"
        aria-label="Asura on X"
        color="neutral"
        variant="ghost"
      />
      <UButton
        to="https://go.91.pt/github"
        target="_blank"
        icon="i-simple-icons-github"
        aria-label="Asura on GitHub"
        color="neutral"
        variant="ghost"
      />
    </template>
  </UFooter>
</template>
