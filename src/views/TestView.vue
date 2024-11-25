<template>
  <div class="min-h-screen flex flex-col bg-gradient-to-b from-blue-50 to-white">
    <header class="bg-white/80 backdrop-blur-sm shadow-sm sticky top-0 z-10">
      <div class="max-w-7xl mx-auto px-4 py-4 flex items-center">
        <button 
          @click="router.back()" 
          class="mr-4 text-gray-600 hover:text-gray-900 transition-colors"
        >
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
          </svg>
        </button>
        <h1 class="text-xl font-semibold text-gray-900">性格测试</h1>
      </div>
    </header>

    <main class="flex-1 container mx-auto px-4 py-6">
      <div class="max-w-lg mx-auto">
        <div class="bg-white rounded-xl shadow-sm p-6 mb-6">
          <div class="mb-6">
            <div class="flex justify-between items-center mb-2">
              <span class="text-sm font-medium text-gray-600">问题 {{ currentQuestionIndex + 1 }}/{{ questions.length }}</span>
              <span class="text-sm font-medium text-primary-600">{{ Math.round((currentQuestionIndex + 1) / questions.length * 100) }}%</span>
            </div>
            <div class="w-full h-2 bg-gray-100 rounded-full overflow-hidden">
              <div 
                class="h-full bg-gradient-to-r from-primary-500 to-blue-500 rounded-full transition-all duration-700 ease-in-out" 
                :style="{ width: `${(currentQuestionIndex + 1) / questions.length * 100}%` }"
              ></div>
            </div>
          </div>

          <transition name="fade" mode="out-in">
            <div :key="currentQuestionIndex">
              <h2 class="text-xl font-semibold mb-6 text-gray-900">{{ currentQuestion.question }}</h2>
              
              <div class="space-y-3">
                <transition-group name="list">
                  <button
                    v-for="(option, idx) in currentQuestion.options"
                    :key="option.value"
                    @click="selectAnswer(option.value)"
                    class="w-full text-left px-6 py-4 rounded-xl border-2 transition-all duration-200"
                    :class="[
                      answers[currentQuestionIndex] === option.value
                        ? 'border-primary-500 bg-primary-50 text-primary-900 scale-[1.02]'
                        : 'border-gray-200 hover:border-primary-200 hover:bg-gray-50 hover:scale-[1.01]'
                    ]"
                    :style="{
                      transitionDelay: `${idx * 50}ms`,
                      transform: `translateY(${idx * 5}px)`
                    }"
                  >
                    {{ option.text }}
                  </button>
                </transition-group>
              </div>
            </div>
          </transition>

          <div class="mt-8 flex justify-between">
            <button 
              v-if="currentQuestionIndex > 0"
              @click="previousQuestion"
              class="px-6 py-3 rounded-xl border-2 border-gray-200 text-gray-700 hover:bg-gray-50 transition-all duration-200 flex items-center"
            >
              <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
              </svg>
              上一题
            </button>
            <button 
              v-if="currentQuestionIndex < questions.length - 1"
              @click="nextQuestion"
              class="px-6 py-3 rounded-xl bg-gradient-to-r from-primary-500 to-blue-500 text-white font-medium hover:shadow-lg hover:shadow-primary-500/30 transition-all duration-200 flex items-center ml-auto"
              :class="{ 'opacity-50 cursor-not-allowed': !answers[currentQuestionIndex] }"
              :disabled="!answers[currentQuestionIndex]"
            >
              下一题
              <svg class="w-5 h-5 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </button>
            <button 
              v-else
              @click="submitTest"
              class="px-6 py-3 rounded-xl bg-gradient-to-r from-primary-500 to-blue-500 text-white font-medium hover:shadow-lg hover:shadow-primary-500/30 transition-all duration-200 ml-auto"
              :class="{ 'opacity-50 cursor-not-allowed': !answers[currentQuestionIndex] }"
              :disabled="!answers[currentQuestionIndex]"
            >
              查看结果
            </button>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const currentQuestionIndex = ref(0)
const answers = ref([])

const questions = [
  {
    question: '当你在社交场合时，你更倾向于：',
    options: [
      { value: 'E', text: '主动与他人交谈，享受社交' },
      { value: 'I', text: '保持安静，更喜欢观察' }
    ]
  },
  {
    question: '在工作环境中，你更喜欢：',
    options: [
      { value: 'E', text: '团队合作，与他人讨论想法' },
      { value: 'I', text: '独立工作，自主思考' }
    ]
  },
  {
    question: '在做决定时，你更依赖：',
    options: [
      { value: 'S', text: '具体的事实和经验' },
      { value: 'N', text: '直觉和可能性' }
    ]
  },
  {
    question: '面对新事物时，你更倾向于：',
    options: [
      { value: 'S', text: '关注细节和实际应用' },
      { value: 'N', text: '探索潜在的可能性和创新' }
    ]
  },
  {
    question: '在处理问题时，你更看重：',
    options: [
      { value: 'T', text: '逻辑和客观分析' },
      { value: 'F', text: '个人价值观和感受' }
    ]
  },
  {
    question: '在做重要决定时，你更倾向于：',
    options: [
      { value: 'T', text: '权衡利弊，理性分析' },
      { value: 'F', text: '考虑对他人的影响和感受' }
    ]
  },
  {
    question: '你更喜欢：',
    options: [
      { value: 'J', text: '按计划行事，提前做好安排' },
      { value: 'P', text: '保持灵活，随机应变' }
    ]
  },
  {
    question: '在日常生活中，你更倾向于：',
    options: [
      { value: 'J', text: '建立规律的作息和习惯' },
      { value: 'P', text: '根据当下心情和状态调整' }
    ]
  },
  {
    question: '当面对压力时，你通常会：',
    options: [
      { value: 'T', text: '分析问题，寻找解决方案' },
      { value: 'F', text: '寻求他人支持和理解' }
    ]
  },
  {
    question: '在空闲时间，你更喜欢：',
    options: [
      { value: 'E', text: '参加社交活动，认识新朋友' },
      { value: 'I', text: '独处，享受个人时光' }
    ]
  }
]

const currentQuestion = computed(() => questions[currentQuestionIndex.value])

const selectAnswer = (value) => {
  answers.value[currentQuestionIndex.value] = value
  setTimeout(() => {
    if (currentQuestionIndex.value < questions.length - 1) {
      nextQuestion()
    } else {
      submitTest() // 最后一题自动提交
    }
  }, 300) // 添加短暂延迟，让用户看到选中效果
}

const previousQuestion = () => {
  if (currentQuestionIndex.value > 0) {
    currentQuestionIndex.value--
  }
}

const nextQuestion = () => {
  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++
  }
}

const submitTest = () => {
  // 计算每个维度的得分
  const dimensions = {
    E: 0, I: 0,
    S: 0, N: 0,
    T: 0, F: 0,
    J: 0, P: 0
  }
  
  // 统计每个选项的次数
  answers.value.forEach(answer => {
    dimensions[answer]++
  })
  
  // 确定每个维度的最终类型
  const result = [
    dimensions.E > dimensions.I ? 'E' : 'I',
    dimensions.S > dimensions.N ? 'S' : 'N',
    dimensions.T > dimensions.F ? 'T' : 'F',
    dimensions.J > dimensions.P ? 'J' : 'P'
  ].join('')
  
  router.push({ 
    name: 'result',
    query: { 
      type: result,
      scores: JSON.stringify(dimensions)  // 传递详细分数
    }
  })
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease-out;
}

.fade-enter-from {
  opacity: 0;
  transform: translateX(20px);
}

.fade-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}

.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease-out;
}

.list-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

/* 添加选中时的弹性动画 */
button {
  transform-origin: center;
  backface-visibility: hidden;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

button:active {
  transform: scale(0.98);
}
</style>
