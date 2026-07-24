<template>
  <div
    class="fixed inset-0 z-[70] flex items-center justify-center bg-black/50 backdrop-blur-sm"
    @click.self="$emit('close')">

    <div class="bg-white dark:bg-slate-800 rounded-2xl shadow-2xl max-w-lg w-full mx-4 overflow-hidden">
      <!-- 头部 -->
      <div class="p-5 border-b border-slate-200 dark:border-slate-700 flex items-center justify-between">
        <h2 class="text-lg font-bold flex items-center gap-2">
          <svg class="w-5 h-5 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
          设置
        </h2>
        <button @click="$emit('close')" class="btn btn-ghost btn-sm btn-circle cursor-pointer text-slate-400 hover:text-slate-600">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- 内容区域 -->
      <div class="p-5 space-y-6 max-h-[60vh] overflow-y-auto custom-scrollbar">

        <!-- 布局设置 -->
        <div class="space-y-3">
          <h3 class="text-sm font-semibold text-slate-500 uppercase tracking-wider">布局</h3>
          <div class="grid grid-cols-2 gap-3">
            <button
              @click="selectLayout('grid')"
              class="btn gap-2 cursor-pointer"
              :class="settings.layout === 'grid' ? 'btn-primary' : 'btn-outline'">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
              </svg>
              网格布局
            </button>
            <button
              @click="selectLayout('list')"
              class="btn gap-2 cursor-pointer"
              :class="settings.layout === 'list' ? 'btn-primary' : 'btn-outline'">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
              </svg>
              列表布局
            </button>
          </div>
        </div>

        <!-- 主题设置 -->
        <div class="space-y-3">
          <h3 class="text-sm font-semibold text-slate-500 uppercase tracking-wider">主题</h3>
          <div class="grid grid-cols-3 gap-3">
            <button
              @click="selectTheme('light')"
              class="btn gap-2 cursor-pointer"
              :class="settings.theme === 'light' ? 'btn-primary' : 'btn-outline'">
              ☀️ 浅色
            </button>
            <button
              @click="selectTheme('dark')"
              class="btn gap-2 cursor-pointer"
              :class="settings.theme === 'dark' ? 'btn-primary' : 'btn-outline'">
              🌙 深色
            </button>
            <button
              @click="selectTheme('auto')"
              class="btn gap-2 cursor-pointer"
              :class="settings.theme === 'auto' ? 'btn-primary' : 'btn-outline'">
              🔄 自动
            </button>
          </div>
        </div>

        <!-- 数据管理 -->
        <div class="space-y-3">
          <h3 class="text-sm font-semibold text-slate-500 uppercase tracking-wider">数据管理</h3>
          <div class="space-y-2">
            <button @click="clearCache" class="btn btn-outline btn-sm w-full gap-2">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
              </svg>
              清除缓存
            </button>
            <button @click="resetSettings" class="btn btn-outline btn-sm w-full gap-2 text-red-500 hover:text-red-600">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
              重置所有设置
            </button>
          </div>
        </div>

        <!-- 快捷键说明 -->
        <div class="space-y-3">
          <h3 class="text-sm font-semibold text-slate-500 uppercase tracking-wider">键盘快捷键</h3>
          <div class="text-xs space-y-2 bg-slate-50 dark:bg-slate-900/50 p-3 rounded-lg">
            <div class="flex justify-between">
              <span>打开搜索</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded">⌘/Ctrl + K</kbd>
            </div>
            <div class="flex justify-between">
              <span>刷新全部</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded">⌘/Ctrl + R</kbd>
            </div>
            <div class="flex justify-between">
              <span>切换布局</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded">⌘/Ctrl + L</kbd>
            </div>
            <div class="flex justify-between">
              <span>关闭弹窗</span>
              <kbd class="px-1.5 py-0.5 bg-slate-200 dark:bg-slate-700 rounded">Esc</kbd>
            </div>
          </div>
        </div>

        <!-- 关于 -->
        <div class="space-y-3 pt-3 border-t border-slate-200 dark:border-slate-700">
          <h3 class="text-sm font-semibold text-slate-500 uppercase tracking-wider">关于</h3>
          <div class="text-xs text-slate-500 space-y-1">
            <p>NewsHub v{{ version }}</p>
            <p>一个现代化的热点聚合平台</p>
       <p class="mt-2">
  <a
    href="https://blog.8818618.xyz"
    target="_blank"
    rel="noopener noreferrer"
    class="text-blue-500 hover:underline"
  >
    访问我的博客 →
  </a>
</p>
          </div>
        </div>
      </div>

      <!-- 底部操作 -->
      <div class="p-4 bg-slate-50 dark:bg-slate-900/50 border-t border-slate-200 dark:border-slate-700 flex justify-end gap-2">
        <button @click="$emit('close')" class="btn btn-ghost">关闭</button>
        <button @click="saveSettings" class="btn btn-primary">保存设置</button>
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['close', 'save'])

const version = ref('1.0.0')

// 设置数据
const settings = ref({
  layout: 'grid',
  theme: 'light',
  pinnedSources: []
})

// 加载保存的设置
const loadSettings = () => {
  const saved = localStorage.getItem('newsHubSettings')
  if (saved) {
    try {
      const parsed = JSON.parse(saved)
      settings.value = { ...settings.value, ...parsed }
    } catch (e) {
      console.error('加载设置失败:', e)
    }
  }

  // 加载置顶源
  const pinned = localStorage.getItem('hot-list-preference')
  if (pinned) {
    try {
      const parsed = JSON.parse(pinned)
      settings.value.pinnedSources = parsed.pinned || []
    } catch (e) {
      console.error('加载置顶数据失败:', e)
    }
  }
}

// 选择布局
const selectLayout = (layout) => {
  settings.value.layout = layout
}

// 选择主题
const selectTheme = (theme) => {
  settings.value.theme = theme
  applyTheme(theme)
}

// 应用主题
const applyTheme = (theme) => {
  let actualTheme = theme

  if (theme === 'auto') {
    actualTheme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
  }

  document.documentElement.setAttribute('data-theme', actualTheme)
  localStorage.setItem('theme', actualTheme)
}

// 清除缓存
const clearCache = () => {
  if (confirm('确定要清除所有缓存数据吗？')) {
    // 清除 localStorage
    const keysToKeep = ['newsHubSettings', 'theme']
    Object.keys(localStorage).forEach(key => {
      if (!keysToKeep.includes(key)) {
        localStorage.removeItem(key)
      }
    })

    // 清除 sessionStorage
    sessionStorage.clear()

    // 刷新页面
    alert('缓存已清除，即将刷新页面')
    window.location.reload()
  }
}

// 重置设置
const resetSettings = () => {
  if (confirm('确定要重置所有设置吗？这将清除您的个性化配置。')) {
    localStorage.removeItem('newsHubSettings')
    localStorage.removeItem('hot-list-preference')
    localStorage.removeItem('searchHistory')

    settings.value = {
      layout: 'grid',
      theme: 'light',
      pinnedSources: []
    }

    applyTheme('light')
    alert('设置已重置')
  }
}

// 保存设置
const saveSettings = () => {
  // 保存到 localStorage
  localStorage.setItem('newsHubSettings', JSON.stringify({
    layout: settings.value.layout,
    theme: settings.value.theme
  }))

  // 保存置顶源
  if (settings.value.pinnedSources.length > 0) {
    localStorage.setItem('hot-list-preference', JSON.stringify({
      pinned: settings.value.pinnedSources
    }))
  }

  emit('save', settings.value)
  emit('close')
}

// 生命周期
onMounted(() => {
  loadSettings()
})
</script>

<style scoped>
.custom-scrollbar::-webkit-scrollbar {
  width: 6px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background: rgba(148, 163, 184, 0.3);
  border-radius: 3px;
}
</style>
