<template>
  <div
    ref="wrapperRef"
    class="collapse-wrapper"
  >
    <div
      v-show="show"
      ref="contentRef"
      class="collapse-content"
    >
      <slot />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, nextTick } from 'vue'

const props = defineProps<{
  expanded: boolean
}>()

const wrapperRef = ref<HTMLElement>()

const contentRef = ref<HTMLElement>()

const show = ref(props.expanded)

const doubleRaf = (fn: () => void) => {
  nextTick(() => nextTick(fn))
}

watch(() => props.expanded, (value, oldValue) => {
  if (oldValue === null) {
    return
  }

  if (value) {
    show.value = true
  }

  nextTick(() => {
    if (!contentRef.value || !wrapperRef.value) {
      return
    }

    const { offsetHeight } = contentRef.value
    if (offsetHeight) {
      const contentHeight = `${offsetHeight}px`
      wrapperRef.value.style.height = value ? '0' : contentHeight

      // use double raf to ensure animation can start
      doubleRaf(() => {
        if (wrapperRef.value) {
          wrapperRef.value.style.height = value ? contentHeight : '0'
        }
      })
    } else {
      onTransitionEnd()
    }
  })
}, { immediate: true })

function onTransitionEnd () {
  if (!props.expanded) {
    show.value = false
  } else if (wrapperRef.value) {
    wrapperRef.value.style.height = ''
  }
}
</script>

<style lang="less" scoped>
.collapse-wrapper {
  overflow: hidden;
  transition: height 0.3s ease-in-out;
}
</style>
