<template>
  <view class="content">
    <view>svg-icon 使用示例(只有在h5中能使用)</view>
    <svg-icon icon-class="dashboard" class="dashboard" />
    <view>vuex测试</view>
    <view @click="getterFunc">获取getter:{{ getterValue }}</view>
    <view class="mt-2">commit</view>
    <view @click="c_openVuexTest">c_openVuexTest</view>
    <view @click="c_closeVuexTest">c_closeVuexTest</view>
    <view class="mt-2">action</view>
    <view @click="openVuexTest">openVuexTest</view>
    <view @click="closeVuexTest">closeVuexTest</view>
    <view>test的store值：{{ store.state.app.test }}</view>
    <view class="mt-4">请求测试</view>
    <view @click="testReq">testReq</view>

    <view class="mt-4">跳转测试  </view>
    <view @click="toNavigateOne">navigateOne</view>
    <view @click="toNavigateTwo">navigateTwo</view>
  </view>
</template>

<script setup>
import { getCurrentInstance, ref } from 'vue'
let title = ref('这是vue3版本')
import { useStore } from 'vuex'
const store = useStore()
/*getter*/
let getterValue = ref(null)
const getterFunc = () => {
  getterValue.value = store.getters.cachedViews
}

//request
import uniRequest from '@/utils/uniRequest'
const testReq = () => {
  uniRequest({
    url: '/integration-front/user/loginOut',
    method: 'post',
    isSBLoading: true,
    isHALoading: true,
    data: {
      test: 1
    },
    isParams: true
  }).then((res) => {
    console.log('我回来了')
  })
}

/*mutations*/
/*建议commit用M_开头 action用A_开头*/
const c_openVuexTest = () => {
  store.commit('app/M_vuex_test', true)
}
const c_closeVuexTest = () => {
  store.commit('app/M_vuex_test', false)
}
/*actions*/
const openVuexTest = () => {
  store.dispatch('app/A_vuex_test', true)
}
const closeVuexTest = () => {
  store.dispatch('app/A_vuex_test', false)
}

//navigate 跳转
const { proxy } = getCurrentInstance()
const toNavigateOne = () => {
  let data = {
    licenseNo: 11,
    name: 'kuanghua'
  }
  proxy.toNavigatePageMixin('/pages/navigateOne/navigateOne', data)
}

const toNavigateTwo = () => {
  let data = {
    licenseNo: 222,
    name: 'kuanghua'
  }
  proxy.toNavigatePageMixin('/pages/navigateTwo/navigateTwo', data)
}
</script>

<style lang="scss" scoped>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
