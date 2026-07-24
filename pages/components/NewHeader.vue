<template>
  <header class="sticky top-0 z-50">
    <!-- 玻璃拟态背景 -->
    <div class="backdrop-blur-xl bg-white/70 dark:bg-slate-800/80 border-b border-slate-200/50 dark:border-slate-700/50">
      <div class="container mx-auto px-4 py-3">
        <div class="flex items-center justify-between gap-4">
          <!-- Logo 和标题 -->
          <div class="flex items-center gap-3 flex-shrink-0">
            <div class="w-10 h-10 bg-gradient-to-br from-blue-500 to-purple-600 rounded-xl flex items-center justify-center shadow-lg shadow-blue-500/30">
              <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
              </svg>
            </div>
            <div class="hidden sm:block">
              <h1 class="text-lg font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
                马老师专用
              </h1>
              <p class="text-xs text-slate-500 dark:text-slate-400">热点聚合新闻</p>
            </div>
          </div>

          <!-- 搜索触发按钮 (移动端) -->
          <button
            @click="$emit('search')"
            class="sm:hidden btn btn-ghost btn-sm btn-circle cursor-pointer"
            title="搜索">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
          </button>

          <!-- 桌面端搜索栏 -->
          <div class="hidden sm:flex flex-1 max-w-md mx-4">
            <div class="relative w-full">
              <input
                type="text"
                placeholder="搜索热点... (Ctrl+K)"
                class="w-full input input-sm input-bordered bg-white/50 dark:bg-slate-700/50 backdrop-blur-sm border-2 border-slate-200 dark:border-slate-600 focus:border-blue-500 focus:outline-none transition-all"
                @focus="$emit('search')" />
              <kbd class="absolute right-2 top-1/2 -translate-y-1/2 text-xs text-slate-400 border border-slate-300 dark:border-slate-600 rounded px-1.5 py-0.5">⌘K</kbd>
            </div>
          </div>

          <!-- 操作按钮组 -->
          <div class="flex items-center gap-1 sm:gap-2">
            <!-- 刷新按钮 -->
            <button
              @click="$emit('refresh')"
              :disabled="loading"
              class="btn btn-ghost btn-sm sm:btn-md btn-circle cursor-pointer"
              :class="{ 'animate-spin': loading }"
              title="刷新全部">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
              </svg>
            </button>

            <!-- 布局切换 -->
            <button
              @click="$emit('toggle-layout')"
              class="btn btn-ghost btn-sm sm:btn-md btn-circle hidden md:block cursor-pointer"
              title="切换布局">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
              </svg>
            </button>

            <!-- 主题切换 -->
            <button
              @click="toggleTheme"
              class="btn btn-ghost btn-sm sm:btn-md btn-circle cursor-pointer"
              :title="currentTheme === 'light' ? '切换到深色' : '切换到浅色'">
              <svg v-if="currentTheme === 'light'" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
              </svg>
              <svg v-else class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
              </svg>
            </button>

            <!-- GitHub -->
<a
  href="https://blog.8818618.xyz"
  target="_blank"
  rel="noopener noreferrer"
  class="btn btn-ghost btn-sm sm:btn-md btn-circle cursor-pointer"
  title="访问我的博客"
>
  <img
    src="https://tc.8818618.xyz/file/1783850983366_image.png"
    alt="我的博客"
    class="w-5 h-5 rounded object-contain"
  >
</a>

            <!-- 更多菜单 -->
            <div class="dropdown dropdown-end">
              <label tabindex="0" class="btn btn-ghost btn-sm sm:btn-md btn-circle cursor-pointer">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z" />
                </svg>
              </label>
              <ul tabindex="0" class="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-52 mt-2">
                <li><a @click="$emit('search')"><span>搜索 (⌘K)</span></a></li>
                <li><a @click="$emit('toggle-layout')"><span>切换布局</span></a></li>
                <li><a @click="toggleTheme"><span>切换主题</span></a></li>
                <li><a href="/api" target="_blank"><span>API 文档</span></a></li>
                <li><a @click="showShortcuts = true"><span>快捷键</span></a></li>
              </ul>
            </div>
          </div>
        </div>

        <!-- 统计信息 -->
        <div class="mt-2 flex items-center gap-3 text-xs text-slate-500 dark:text-slate-400">
          <span class="flex items-center gap-1">
            <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
            在线: {{ sourceCount }} 个数据源
          </span>
          <span v-if="loading" class="text-blue-500">刷新中...</span>
        </div>
      </div>
    </div>

    <!-- 快捷键提示弹窗 -->
    <div v-if="showShortcuts" class="fixed inset-0 z-[60] flex items-center justify-center bg-black/50 backdrop-blur-sm" @click="showShortcuts = false">
      <div class="bg-white dark:bg-slate-800 rounded-2xl p-6 max-w-md w-full mx-4 shadow-2xl" @click.stop>
        <h3 class="text-lg font-bold mb-4">键盘快捷键</h3>
        <div class="space-y-2 text-sm">
          <div class="flex justify-between py-2 border-b border-slate-200 dark:border-slate-700">
            <span>打开搜索</span>
            <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-700 rounded">⌘/Ctrl + K</kbd>
          </div>
          <div class="flex justify-between py-2 border-b border-slate-200 dark:border-slate-700">
            <span>刷新全部</span>
            <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-700 rounded">⌘/Ctrl + R</kbd>
          </div>
          <div class="flex justify-between py-2 border-b border-slate-200 dark:border-slate-700">
            <span>切换布局</span>
            <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-700 rounded">⌘/Ctrl + L</kbd>
          </div>
          <div class="flex justify-between py-2 border-b border-slate-200 dark:border-slate-700">
            <span>关闭弹窗</span>
            <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-700 rounded">Esc</kbd>
          </div>
        </div>
        <button @click="showShortcuts = false" class="btn btn-primary btn-block mt-4">明白</button>
      </div>
    </div>
  </header>
</template>

<script setup>
const props = defineProps({
  sourceCount: {
    type: Number,
    default: 0
  },
  loading: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['refresh', 'search', 'toggle-layout'])

// 主题管理
const currentTheme = ref('light')
const showShortcuts = ref(false)

const toggleTheme = () => {
  const newTheme = currentTheme.value === 'light' ? 'dark' : 'light'
  currentTheme.value = newTheme
  document.documentElement.setAttribute('data-theme', newTheme)
  localStorage.setItem('theme', newTheme)
}

// 初始化主题
onMounted(() => {
  const savedTheme = localStorage.getItem('theme') || 'light'
  currentTheme.value = savedTheme
  document.documentElement.setAttribute('data-theme', savedTheme)

  // 键盘快捷键
  const handleKeydown = (e) => {
    const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0
    const modifier = isMac ? e.metaKey : e.ctrlKey

    if (modifier && e.key === 'k') {
      e.preventDefault()
      emit('search')
    } else if (modifier && e.key === 'r') {
      e.preventDefault()
      emit('refresh')
    } else if (modifier && e.key === 'l') {
      e.preventDefault()
      emit('toggle-layout')
    } else if (e.key === 'Escape') {
      showShortcuts.value = false
    }
  }

  window.addEventListener('keydown', handleKeydown)
  onUnmounted(() => {
    window.removeEventListener('keydown', handleKeydown)
  })
})
</script>
