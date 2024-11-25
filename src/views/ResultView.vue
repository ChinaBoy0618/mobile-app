<template>
  <div class="min-h-screen flex flex-col bg-gradient-to-b from-blue-50 to-white">
    <header class="bg-white/80 backdrop-blur-sm shadow-sm sticky top-0 z-10">
      <div class="max-w-7xl mx-auto px-4 py-4">
        <h1 class="text-xl font-semibold text-gray-900">测试结果</h1>
      </div>
    </header>

    <main class="flex-1 container mx-auto px-4 py-6">
      <div class="max-w-2xl mx-auto">
        <!-- 性格类型卡片 -->
        <transition 
          appear
          name="fade-up"
          @before-enter="beforeEnter"
          @enter="enter"
        >
          <div class="bg-white rounded-xl shadow-sm p-8 mb-8">
            <div class="text-center mb-8">
              <div class="inline-flex items-center justify-center w-20 h-20 rounded-full bg-primary-100 mb-4">
                <svg class="w-10 h-10 text-primary-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
                </svg>
              </div>
              <h2 class="text-4xl font-bold mb-2 bg-gradient-to-r from-primary-600 to-blue-600 bg-clip-text text-transparent">
                {{ personalityType }}
              </h2>
              <p class="text-lg text-gray-600">{{ personalityDescription }}</p>
            </div>
          </div>
        </transition>

        <!-- 特征分析 -->
        <div class="space-y-8">
          <transition-group 
            appear
            name="list" 
            tag="div" 
            class="space-y-8"
          >
            <div 
              v-for="(trait, index) in traits" 
              :key="trait.title"
              class="bg-white rounded-xl shadow-sm p-6"
              :style="{ transitionDelay: `${index * 100}ms` }"
            >
              <div class="flex items-center justify-between mb-4">
                <h3 class="text-lg font-semibold text-gray-900">{{ trait.title }}</h3>
                <span class="text-sm font-medium text-primary-600">{{ Math.round(trait.percentage) }}%</span>
              </div>
              
              <div class="relative mb-4">
                <div class="flex justify-between text-sm text-gray-500 mb-2">
                  <span>{{ trait.leftLabel }}</span>
                  <span>{{ trait.rightLabel }}</span>
                </div>
                <div class="w-full h-3 bg-gray-100 rounded-full overflow-hidden">
                  <div 
                    class="h-full bg-gradient-to-r from-primary-500 to-blue-500 rounded-full transition-all duration-1000 ease-out"
                    :style="{ width: '0%' }"
                    ref="progressBars"
                  ></div>
                </div>
              </div>
              
              <p class="text-gray-600 text-sm">{{ trait.description }}</p>
            </div>
          </transition-group>
        </div>

        <!-- 操作按钮 -->
        <div class="mt-8 space-y-4">
          <button 
            @click="shareResult"
            class="w-full py-4 px-6 rounded-xl bg-gradient-to-r from-primary-500 to-blue-500 text-white font-semibold text-lg shadow-lg shadow-primary-500/30 hover:shadow-xl hover:shadow-primary-500/40 transform hover:-translate-y-0.5 transition-all duration-200 flex items-center justify-center"
          >
            <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z" />
            </svg>
            分享结果
          </button>
          
          <button 
            @click="retakeTest"
            class="w-full py-4 px-6 rounded-xl border-2 border-gray-200 text-gray-700 font-medium hover:bg-gray-50 transition-all duration-200"
          >
            重新测试
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const personalityTypes = {
  'ISTJ': '尽责的检查者',
  'ISFJ': '温暖的守护者',
  'INFJ': '富有洞察力的理想主义者',
  'INTJ': '富有远见的策略家',
  'ISTP': '灵活的分析者',
  'ISFP': '敏感的艺术家',
  'INFP': '富有同情心的理想主义者',
  'INTP': '创新的思考者',
  'ESTP': '灵活的实干家',
  'ESFP': '活力四射的表演者',
  'ENFP': '热情的梦想家',
  'ENTP': '大胆的创新者',
  'ESTJ': '高效的管理者',
  'ESFJ': '热心的照顾者',
  'ENFJ': '富有魅力的引导者',
  'ENTJ': '果断的领导者'
}

const personalityType = computed(() => {
  const type = route.query.type
  return `${type} - ${personalityTypes[type] || '未知类型'}`
})

const personalityDescription = computed(() => {
  const type = route.query.type
  // 这里可以根据不同的类型返回详细的描述
  return '这是一段关于你性格类型的详细描述...'
})

const scores = computed(() => {
  try {
    return JSON.parse(route.query.scores || '{}')
  } catch {
    return {}
  }
})

const traits = computed(() => {
  const type = route.query.type
  const dimensionScores = scores.value
  const total = 3 // 每个维度的总题数
  
  return [
    {
      title: '能量来源',
      leftLabel: '外向 (E)',
      rightLabel: '内向 (I)',
      percentage: (dimensionScores.E / total) * 100,
      description: type.includes('E') ? 
        '你倾向于从外部世界获取能量，喜欢与人交往和互动。你享受社交活动，善于表达自己的想法。' : 
        '你倾向于从内心世界获取能量，需要独处时间来恢复精力。你善于倾听和观察，享受独处的时光。'
    },
    {
      title: '信息获取',
      leftLabel: '实感 (S)',
      rightLabel: '直觉 (N)',
      percentage: (dimensionScores.S / total) * 100,
      description: type.includes('S') ? 
        '你更相信具体的事实和经验，注重现实和细节。你是个务实的人，善于处理具体问题。' : 
        '你更相信直觉和可能性，喜欢思考未来和创新。你善于发现潜在机会，喜欢探索新的可能。'
    },
    {
      title: '决策方式',
      leftLabel: '思考 (T)',
      rightLabel: '感受 (F)',
      percentage: (dimensionScores.T / total) * 100,
      description: type.includes('T') ? 
        '你更依赖逻辑和客观分析来做决定。你重视公平和效率，善于理性分析问题。' : 
        '你更依赖个人价值观和感受来做决定。你重视和谐与人际关系，善于理解他人感受。'
    },
    {
      title: '生活方式',
      leftLabel: '判断 (J)',
      rightLabel: '知觉 (P)',
      percentage: (dimensionScores.J / total) * 100,
      description: type.includes('J') ? 
        '你喜欢有计划和条理的生活方式。你善于组织和规划，喜欢按部就班地完成目标。' : 
        '你喜欢灵活和自发的生活方式。你善于适应变化，享受探索和新的体验。'
    }
  ]
})

const progressBars = ref([])

const beforeEnter = (el) => {
  el.style.opacity = 0
  el.style.transform = 'translateY(50px)'
}

const enter = (el, done) => {
  setTimeout(() => {
    el.style.transition = 'all 0.6s ease-out'
    el.style.opacity = 1
    el.style.transform = 'translateY(0)'
  }, 0)
}

const shareResult = () => {
  // 实现分享功能
  alert('分享功能开发中...')
}

const retakeTest = () => {
  router.push('/test')
}

onMounted(() => {
  if (!route.query.type) {
    router.push('/')
  }
  
  // 延迟显示进度条
  setTimeout(() => {
    progressBars.value.forEach((bar, index) => {
      if (bar) {
        bar.style.width = `${traits.value[index].percentage}%`
      }
    })
  }, 500)
})
</script>

<style scoped>
.fade-up-enter-active {
  transition: all 0.6s ease-out;
}

.fade-up-enter-from {
  opacity: 0;
  transform: translateY(50px);
}

.list-enter-active {
  transition: all 0.6s ease-out;
}

.list-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

/* 添加按钮动画 */
button {
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

button:active {
  transform: scale(0.98);
}
</style>
