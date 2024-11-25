<template>
  <div class="min-h-screen flex flex-col bg-gradient-to-b from-blue-50 to-white">
    <header class="bg-white/80 backdrop-blur-sm shadow-sm sticky top-0 z-10">
      <div class="max-w-7xl mx-auto px-4 py-4">
        <h1 class="text-xl font-semibold text-gray-900">性格测试</h1>
      </div>
    </header>

    <main class="flex-1 container mx-auto px-4 py-6">
      <div class="max-w-lg mx-auto mt-8">
        <div class="text-center mb-12">
          <div class="w-24 h-24 mx-auto mb-6 rounded-full bg-primary-100 flex items-center justify-center">
            <svg class="w-12 h-12 text-primary-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5"></path>
            </svg>
          </div>
          <h2 class="text-3xl font-bold mb-4 bg-gradient-to-r from-primary-600 to-blue-600 bg-clip-text text-transparent">
            探索你的性格类型
          </h2>
          <p class="text-gray-600 mb-8 text-lg">
            通过这个有趣的测试，发现真实的自己
          </p>
        </div>
        
        <div class="space-y-6">
          <div class="bg-white rounded-xl shadow-sm p-6 space-y-4">
            <div class="flex items-center space-x-4">
              <div class="w-10 h-10 rounded-full bg-primary-100 flex items-center justify-center flex-shrink-0">
                <span class="text-primary-600 font-semibold">1</span>
              </div>
              <div>
                <h3 class="font-semibold text-gray-900">回答简单问题</h3>
                <p class="text-gray-600">10个精心设计的问题，准确评估你的性格特征</p>
              </div>
            </div>
          </div>
          
          <div class="bg-white rounded-xl shadow-sm p-6 space-y-4">
            <div class="flex items-center space-x-4">
              <div class="w-10 h-10 rounded-full bg-primary-100 flex items-center justify-center flex-shrink-0">
                <span class="text-primary-600 font-semibold">2</span>
              </div>
              <div>
                <h3 class="font-semibold text-gray-900">获取详细分析</h3>
                <p class="text-gray-600">了解你的性格类型和独特特征</p>
              </div>
            </div>
          </div>
          
          <div class="bg-white rounded-xl shadow-sm p-6 space-y-4">
            <div class="flex items-center space-x-4">
              <div class="w-10 h-10 rounded-full bg-primary-100 flex items-center justify-center flex-shrink-0">
                <span class="text-primary-600 font-semibold">3</span>
              </div>
              <div>
                <h3 class="font-semibold text-gray-900">分享你的结果</h3>
                <p class="text-gray-600">与朋友分享你的性格特点</p>
              </div>
            </div>
          </div>
        </div>

        <div class="mt-12">
          <button 
            @click="startTest"
            class="w-full py-4 px-6 rounded-xl bg-gradient-to-r from-primary-500 to-blue-500 text-white font-semibold text-lg shadow-lg shadow-primary-500/30 hover:shadow-xl hover:shadow-primary-500/40 transform hover:-translate-y-0.5 transition-all duration-200"
          >
            开始测试
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()

const startTest = () => {
  router.push('/test')
}
</script>
