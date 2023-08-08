<script setup lang="ts">
import { useMemberStore } from '@/stores/modules/member'
import { http } from '@/utils/http'

const store = useMemberStore()
const click = () => {
  store.setProfile({ name: 'zs', age: 18 })
}

const onGet = async () => {
  // uni.request({
  //   url: '/home/banner',
  //   method: 'GET',
  //   data: {
  //     distributionSite: 1,
  //   },
  //   success: (res) => {
  //     console.log(res)
  //   },
  // })
  const res = await http({
    url: '/home/banner',
    method: 'GET',
    data: {
      distributionSite: 1,
    },
  })
  console.log(res)
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
  <button @tap="onGet" type="primary">click</button>
</template>

<style lang="scss">
//
</style>
