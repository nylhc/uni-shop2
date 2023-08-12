<script setup lang="ts">
import CustomNavbar from './components/CustomNavbar.vue'
import CategoryPanel from './components/CategoryPanel.vue'
import HotPanel from './components/HotPanel.vue'
import PageSkeleton from './components/PageSkeleton.vue'
import type { BannerItem, CategoryItem, HotItem } from '@/types/home'
import { onMounted, ref } from 'vue'
import {
  getHomeBannerAPI,
  getHomeCategoryAPI,
  getHomeHotAPI,
} from '@/services/home'
import type { XtxGuessInstance } from '@/types/components'

const bannerItem = ref<BannerItem[]>([])
const categoryItem = ref<CategoryItem[]>([])
const hotItem = ref<HotItem[]>([])

const getBanner = async () => {
  const homeBannerRes = await getHomeBannerAPI()
  console.log('获取首页-广告区域成功', homeBannerRes)
  bannerItem.value = homeBannerRes.result
}

const getHomeCategory = async () => {
  const homeCategoryRes = await getHomeCategoryAPI()
  console.log('获取首页-前台分类成功', homeCategoryRes)
  categoryItem.value = homeCategoryRes.result
}

const getHomeHot = async () => {
  const homeHotRes = await getHomeHotAPI()
  console.log('获取首页-热门推荐成功', homeHotRes)
  hotItem.value = homeHotRes.result
}

const isLoading = ref(false)
onMounted(async () => {
  isLoading.value = true
  try {
    await Promise.all([getBanner(), getHomeCategory(), getHomeHot()])
  } catch (error) {
    console.error('获取数据失败', error)
  } finally {
    isLoading.value = false
  }
})

// 获取猜你喜欢组件实例
const guessRef = ref<XtxGuessInstance>()

// 滚动触底事件
const onScrolltolower = () => {
  console.log('触底了')
  guessRef.value?.getMore()
}

// 下拉刷新状态
const isTriggered = ref(false)
// 自定义下拉刷新被触发
const onRefresherrefresh = async () => {
  // 开启动画
  isTriggered.value = true
  // 重置猜你喜欢组件数据
  guessRef.value?.resetData() // 加载数据
  await Promise.all([
    getHomeBannerAPI(),
    getHomeCategoryAPI(),
    getHomeHotAPI(),
    guessRef.value?.getMore(),
  ]) // 关闭动画
  isTriggered.value = false
}
</script>

<template>
  <custom-navbar />
  <!-- 滚动容器 -->
  <scroll-view
    @scrolltolower="onScrolltolower"
    refresher-enabled
    @refresherrefresh="onRefresherrefresh"
    :refresher-triggered="isTriggered"
    class="scroll-view"
    scroll-y
  >
    <page-skeleton v-if="isLoading" />
    <template else>
      <XtxSwiper :list="bannerItem" />
      <category-panel :list="categoryItem" />
      <hot-panel :list="hotItem" />
      <!-- 猜你喜欢 -->
      <XtxGuess ref="guessRef" />
    </template>
  </scroll-view>
</template>

<style lang="scss">
page {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: #f5f5f5;
  .scroll-page {
    flex: 1;
  }
}
</style>
