<script setup lang='ts'>
const { data: page } = await useAsyncData('investors', () => queryCollection('investors').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

useSeoMeta({
  titleTemplate: '',
  title,
  ogTitle: title,
  description,
  ogDescription: description
})
</script>

<template>
  <div>
    <div v-if="page">
      <UPageHero
        v-bind="{
          title: page.hero.title,
          description: page.hero.description,
          orientation: page.hero.orientation,
          links: page.hero.links
        }"
      >
        <template #title>
          <MDC
            :value="page.hero.title"
            unwrap="p"
            class=""
          />
        </template>
        <UPageCard variant="subtle">
          <InvestorVideo />
        </UPageCard>
      </UPageHero>

      <UPageCTA
        v-if="page.partners && page.partners.length"
        title="Trusted Partners & Sponsors"
        variant="subtle"
        :ui="{
          container: 'px-0 lg:px-0'
        }"
      >
        <UMarquee
          :repeat="10"
          pause-on-hover
        >
          <div
            v-for="(partner, ind) of page.partners"
            :key="ind"
          >
            <UTooltip :text="partner.name">
              <ULink
                :to="partner.url"
                target="_blank"
              >
                <NuxtImg
                  :src="partner.image.src"
                  :alt="partner.image.alt"
                  class="h-14 object-cover"
                />
              </ULink>
            </UTooltip>
          </div>
        </UMarquee>
      </UPageCTA>

      <MotionSection
        v-for="(section, index) in page.sections"
        :key="index"
        v-bind="{ ...section }"
      >
        <NuxtImg
          v-if="section.image && section.image.src"
          :src="section.image?.src"
          :alt="section.image?.alt"
        />
        <ImagePlaceholder
          v-else
          class="size-full"
        />
      </MotionSection>

      <UPageCTA
        v-bind="page.cta"
        variant="soft"
        class="overflow-hidden"
      >
      </UPageCTA>
    </div>
  </div>
</template>

<style scoped>

</style>