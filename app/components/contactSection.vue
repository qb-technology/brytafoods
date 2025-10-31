<script lang="ts" setup>
import { breakpointsTailwind, useBreakpoints } from '@vueuse/core'
import type { PageSectionProps } from '@nuxt/ui'

interface CPageSectionProps extends PageSectionProps {
  socials: {
    address: string
    email: string
    phone: string
    instagram: string
    x?: string
    linkedin?: string
    facebook: string
    youtube?: string
  }
}

const props = defineProps<CPageSectionProps>()

const breakpoints = useBreakpoints(breakpointsTailwind)

const mobile = breakpoints.smallerOrEqual('sm') // mobile
const tablet = breakpoints.smallerOrEqual('lg') // tablet
const large = breakpoints.greaterOrEqual('lg') // large

const bg_image = computed(() => {
  let _ = ''
  if (mobile.value) {
    _ = '/contact-3-min.png'
  } else if (tablet.value) {
    _ = '/contact-2-min.png'
  } else {
    _ = '/contact-1.png'
  }
  return {
    'background-image': `url(${_})`
  }
})

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
  <Motion
    v-bind="motion"
    :style="bg_image"
    class="bg-cover"
  >
    <div
      class="bg-neutral-950/60 text-white py-16 sm:py-24 lg:py-32"
    >
      <u-container>
        <div class="bg-primary-800/50 p-16 space-y-6">
          <!-- <MDC
          v-if="props.title"
          :value="props.title"
          unwrap="p"
          class=""
        /> -->
          <h1 class="text-3xl sm:text-4xl lg:text-5xl text-pretty tracking-tight font-bold">
            {{ title }}
          </h1>
          <p class="text-base sm:text-lg">
            {{ description }}
          </p>
          <div class="grid md:flex gap-5 md:gap-32">
            <div class="flex flex-col gap-2">
              <u-button
                variant="ghost"
                color="neutral"
                :to="socials.phone"
                icon="i-lucide-phone"
                label="Telephone"
                :ui="{
                  label: 'text-white',
                  leadingIcon: 'text-white'
                }"
              />
              <u-button
                variant="ghost"
                color="neutral"
                :to="socials.email"
                icon="i-lucide-mail"
                label="E-mail"
                :ui="{
                  label: 'text-white',
                  leadingIcon: 'text-white'
                }"
              />
              <u-button
                variant="ghost"
                color="neutral"
                icon="i-lucide-map"
                :label="socials.address"
                :ui="{
                  label: 'text-white',
                  leadingIcon: 'text-white'
                }"
              />
            </div>

            <div class="flex md:flex-col items-center gap-2">
              <u-button
                variant="solid"
                color="neutral"
                :to="socials.instagram"
                icon="i-lucide-instagram"
              />
              <u-button
                variant="solid"
                color="neutral"
                :to="socials.x"
                icon="i-simple-icons-x"
              />
              <u-button
                variant="solid"
                color="neutral"
                :to="socials.linkedin"
                icon="i-lucide-linkedin"
              />
              <u-button
                variant="solid"
                color="neutral"
                :to="socials.youtube"
                icon="i-lucide-youtube"
              />
            </div>
          </div>
        </div>
      </u-container>
    </div>
  </Motion>
</template>

<style scoped>

</style>