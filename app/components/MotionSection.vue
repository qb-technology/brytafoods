<script lang="ts" setup>
import type { PageSectionProps } from '@nuxt/ui'

const props = defineProps<PageSectionProps>()

const motion = {
  initial: {
    opacity: 0,
    filter: 'blur(20px)',
    scale: 1.1,
    transform: 'translateY(20px)'
  },
  'while-in-view': { opacity: 1, transform: 'translateY(0)' },
  animate: {
    scale: 1,
    opacity: 1,
    filter: 'blur(0px)'
  },
  transition: {
    duration: 0.6,
    delay: 0.3
  },
  'in-view-options': { once: true }
}
</script>

<template>
  <Motion v-bind="motion">
    <u-page-section v-bind="props">
      <template #title>
        <MDC
          v-if="props.title"
          :value="props.title"
          unwrap="p"
          class=""
        />
      </template>
      <slot />
    </u-page-section>
  </Motion>
</template>

<style scoped>

</style>