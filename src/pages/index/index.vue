<script setup lang="ts">
import { useMemberStore } from '@/stores/modules/member'

const store = useMemberStore()
const click = () => {
  store.setProfile({ name: 'zs', age: 18 })
}
</script>

<template>
  <view class="index">
    <uni-collapse>
      <uni-collapse-item title="默认开启" :open="true"
        ><text>折叠内容</text>
      </uni-collapse-item>
      <uni-collapse-item title="折叠内容">
        <text>折叠内容</text>
      </uni-collapse-item>
      <uni-collapse-item title="禁用状态" disabled>
        <text>折叠内容</text>
      </uni-collapse-item>
    </uni-collapse>
  </view>
  <view>{{ store.profile }}</view>
  <button @tap="click">click</button>
</template>

<style lang="scss">
//
</style>
