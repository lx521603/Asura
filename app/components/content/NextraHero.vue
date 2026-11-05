<script setup lang="ts">
const props = defineProps<{
  title?: string
  rotatingWords?: string[] 
  description?: string
}>()

// 按字符拆分，实现中文逐字跳出
const titleChars = computed(() => props.title ? props.title.split('') : [])
</script>

<template>
  <section class="relative w-full overflow-hidden pt-20 pb-12 sm:pt-16 sm:pb-8">
    <!-- 1. 背景网格和光晕 -->
    <div class="absolute inset-0 z-0 pointer-events-none">
      <div class="absolute inset-0 bg-[linear-gradient(to_right,#80808012_1px,transparent_1px),linear-gradient(to_bottom,#80808012_1px,transparent_1px)] bg-[size:24px_24px]"></div>
      <div class="absolute inset-0 bg-gradient-to-b from-white via-transparent to-white dark:from-gray-950 dark:via-transparent dark:to-gray-950"></div>
    </div>
    <div class="absolute top-0 left-1/4 w-96 h-96 bg-purple-500/30 rounded-full mix-blend-multiply filter blur-3xl opacity-70 animate-blob"></div>
    <div class="absolute top-0 right-1/4 w-96 h-96 bg-cyan-500/30 rounded-full mix-blend-multiply filter blur-3xl opacity-70 animate-blob animation-delay-2000"></div>

    <!-- 2. 前景内容 -->
    <div class="relative z-10 mx-auto max-w-4xl px-4 text-center sm:px-6 lg:px-8">
      
      <!-- Headline 插槽 -->
      <div class="mb-6 flex justify-center min-h-[40px]">
        <slot name="headline"></slot>
      </div>

      <!-- 👇 你最初认可的完美布局 👇 -->
      <h1 class="text-4xl font-extrabold tracking-tight text-gray-900 dark:text-white sm:text-6xl mb-6 flex flex-wrap justify-center gap-x-3">
        
        <!-- 逐字跳出 (速度调至 200ms) -->
        <span 
          v-for="(char, index) in titleChars" 
          :key="index"
          class="inline-block opacity-0 animate-fade-in-up"
          :style="{ animationDelay: `${index * 300}ms` }"
        >
          {{ char === ' ' ? '\u00A0' : char }}
        </span>
        
        <!-- 👇 你最初给的轮换代码（完全保留） 👇 -->
        <span v-if="rotatingWords && rotatingWords.length > 0" class="inline-flex flex-col h-[1.2em] overflow-hidden align-bottom relative">
          <div class="animate-rotate-words">
            <span 
              v-for="(word, index) in rotatingWords" 
              :key="index" 
              class="block h-[1.2em] text-transparent bg-clip-text bg-gradient-to-r from-purple-600 to-cyan-600 dark:from-purple-400 dark:to-cyan-400"
            >
              {{ word }}
            </span>
          </div>
        </span>
      </h1>

      <!-- Description 插槽 -->
      <div class="mx-auto max-w-2xl text-lg text-gray-600 dark:text-gray-300 mb-10 opacity-0 animate-fade-in-up" style="animation-delay: 600ms;">
        <slot name="description">
          {{ description }}
        </slot>
      </div>

      <!-- Links 插槽 -->
      <div class="flex flex-col sm:flex-row gap-4 justify-center items-center flex-wrap opacity-0 animate-fade-in-up" style="animation-delay: 800ms;">
        <slot name="links">
          <UButton size="lg" to="/getting-started" label="开始阅读" trailing-icon="i-lucide-arrow-right" />
          <UButton size="lg" to="/blog" variant="outline" label="查看 Blog" icon="i-simple-icons-blogger" />
        </slot>
      </div>

    </div>
  </section>
</template>

<style scoped>
/* 光晕动画 */
@keyframes blob {
  0% { transform: translate(0px, 0px) scale(1); }
  33% { transform: translate(30px, -50px) scale(1.1); }
  66% { transform: translate(-20px, 20px) scale(0.9); }
  100% { transform: translate(0px, 0px) scale(1); }
}
.animate-blob { animation: blob 7s infinite; }
.animation-delay-2000 { animation-delay: 2s; }

/* 逐字浮现动画 */
@keyframes fade-in-up {
  0% { opacity: 0; transform: translateY(20px); }
  100% { opacity: 1; transform: translateY(0); }
}
.animate-fade-in-up {
  animation: fade-in-up 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

/* 👇 你最初给的轮换动画（完全保留） 👇 */
@keyframes rotate-words {
  0%, 20% { transform: translateY(0); }
  25%, 45% { transform: translateY(-1.2em); }
  50%, 70% { transform: translateY(-2.4em); }
  75%, 95% { transform: translateY(-3.6em); }
  100% { transform: translateY(0); }
}
.animate-rotate-words {
  animation: rotate-words 8s infinite cubic-bezier(0.16, 1, 0.3, 1);
}
</style>