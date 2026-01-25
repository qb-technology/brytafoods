<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

const active = ref()
const navLinks: NavigationMenuItem[] = [
  {
    label: 'About Us',
    onSelect: () => {
      navigateTo('/#about-us')
      // active.value = 'item-0'
    }
  },
  {
    label: 'Our Product',
    // to: '/#our-product',
    onSelect: () => {
      navigateTo('/#our-product')
      // active.value = 'item-0'
    }
  },
  {
    label: 'Recipes',
    to: '/recipes',
  },
  {
    label: 'Contact Us',
    // to: '/#contact-us',
    onSelect: () => {
      navigateTo('/#contact-us')
      // active.value = 'item-0'
    }
  },
]
</script>

<template>
  <UHeader
    mode="slideover"
  >
    <template #left>
      <NuxtLink to="/">
        <AppLogo class="size-10 shrink-0" />
      </NuxtLink>
    </template>

    <UNavigationMenu
      v-model="active"
      :items="navLinks"
      variant="link"
      highlight
      highlight-color="primary"
    >
    </UNavigationMenu>

    <template #right>
      <UColorModeButton />

      <UButton
        color="primary"
        variant="solid"
        size="xl"
        to="/partners_investors_sponsors"
        label="Learn More"
      />
    </template>

    <template #body>
      <UNavigationMenu
        :items="navLinks"
        orientation="vertical"
        class="-mx-2.5"
      />

      <USeparator class="my-6" />

      <UButton
        label="Learn More"
        color="primary"
        variant="solid"
        to="/partners_investors_sponsors"
        block
        class="mb-3"
      />
      <!-- <UButton
        label="Sign in"
        color="neutral"
        variant="subtle"
        to="/login"
        block
        class="mb-3"
      />
      <UButton
        label="Sign up"
        color="neutral"
        to="/signup"
        block
      /> -->
    </template>
  </UHeader>
</template>
