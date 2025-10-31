<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

const navLinks: NavigationMenuItem[] = [
  {
    label: 'About Us',
    to: '/about-us'
  },
  {
    label: 'Our Product',
    to: '#',
  },
  {
    label: 'Recipes',
    to: '/recipes',
  },
  {
    label: 'Contact Us',
    to: '/contact-us'
  },
]
</script>

<template>
  <UMain>
    <AppHeader :links="navLinks" />
    <slot />
    <AppFooter />
  </UMain>
</template>

<style scoped></style>
