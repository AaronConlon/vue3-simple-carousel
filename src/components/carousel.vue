<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const left = ref(0)
const imgList = [
  'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?q=80&w=2673&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  'https://images.unsplash.com/photo-1439405326854-014607f694d7?q=80&w=2670&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  'https://images.unsplash.com/photo-1471922694854-ff1b63b20054?q=80&w=2672&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
]

// 处理点击事件
const handleClick = (index) => {
  // 计算起始位置
  left.value = -800 * index
}
// 暂停
const onMouseEnter = () => {
  clearInterval(timer)
}

// 定时器
let timer = null
// 根据 flag 判断向前还是向后播放
let positive = true
const autoPlay = () => {
  clearInterval(timer)
  timer = setInterval(() => {
    if (left.value === -(imgList.length - 1) * 800) {
      positive = false
    } else if (left.value === 0) {
      positive = true
    }
    if (positive) {
      left.value -= 800
    } else {
      left.value += 800
    }
  }, 2000)
}

// 初始化
onMounted(autoPlay)

// 销毁
onUnmounted(() => {
  clearInterval(timer)
})

</script>

<template>
  <div @mouseenter="onMouseEnter" @mouseleave="() => autoPlay()" class="container group select-none" :style="{
    '--left': left + 'px'
  }">
    <div class="wrap">
      <img v-for="url in imgList" :src="url" :key="url" alt="" />
    </div>

    <ul
      class="absolute z-10 inset-x-0 -bottom-12  group-hover:bottom-0 transition-all flex items-center justify-center gap-4 p-4">
      <li :class="['w-3 h-3 rounded-full cursor-pointer', left === index * -800 ? 'bg-blue-500' : 'bg-sky-50/40']"
        v-for="(_, index) in 3" :key="index" @click="handleClick(index)">
      </li>
    </ul>
    <div
      class="absolute px-2 -left-12 inset-y-0 flex items-center transition-all delay-100 ease-in-out group-hover:left-0 bg-transparent group-hover:bg-gray-400/10"
      :style="{
        opacity: left === 0 ? 0.2 : 1
      }">
      <span class="cursor-pointer bg-blue-50 rounded-full p-1" @click="() => {
        if (left === 0) return
        left += 800
      }">
        < </span>
    </div>
    <div
      class="absolute px-2 -right-12 inset-y-0 flex items-center transition-all delay-100 ease-in-out group-hover:right-0 bg-transparent group-hover:bg-gray-400/10"
      :style="{
        opacity: left === -(imgList.length - 1) * 800 ? 0.2 : 1
      }">
      <span class="cursor-pointer bg-blue-50 rounded-full p-1" @click="() => {
        if (left === -(imgList.length - 1) * 800) return
        left -= 800
      }">
        >
      </span>
    </div>
  </div>

</template>

<style scoped>
/* 容器大小 */
.container {
  height: 300px;
  width: 800px;
  overflow: hidden;
  position: relative;
  margin: 100px auto;
}

/* .wrap */
.wrap {
  position: relative;
  left: var(--left);
  width: 2400px;
  transition: left 0.6s ease-in-out;
}

/* 图片大小 */
.wrap img {
  width: 800px;
  float: left;
  height: 300px;
  display: block;
}
</style>