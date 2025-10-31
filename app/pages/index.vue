<script setup lang='ts'>
const { data: page } = await useAsyncData('index', () => queryCollection('index').first())

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
      <hero-slider
        v-if="page.hero"
        v-model="page.hero"
      />

      <UPageCTA
        v-if="page.partners && page.partners.length"
        title="Trusted Partners"
        variant="naked"
      >
        <UMarquee
          :repeat="10"
          pause-on-hover
        >
          <UPageCard
            v-for="(partner, ind) of page.partners"
            :key="ind"
            variant="naked"
          >
            <UTooltip :text="partner.name">
              <ULink
                :to="partner.url"
                target="_blank"
              >
                <NuxtImg
                  :src="partner.image.src"
                  :alt="partner.image.alt"
                />
              </ULink>
            </UTooltip>
          </UPageCard>
        </UMarquee>
      </UPageCTA>

      <MotionSection
        v-for="(section, index) in page.sections"
        :key="index"
        :title="section.title"
        :description="section.description"
        :orientation="section.orientation"
        :reverse="section.reverse"
        :features="section.features"
        :links="section.links"
        :headline="section.headline"
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

      <MotionSection
        :title="page.team.title"
        :description="page.team.description"
        :icon="page.team.icon"
        :orientation="page.team.orientation"
        :reverse="page.team.reverse"
      >
        <UPageGrid class="lg:grid-cols-4">
          <UPageCard
            v-for="({ image, ...item }, index) in page.team.items"
            :key="index"
            v-bind="{
              ...item,
              variant: item.variant || 'soft',
              reverse: item.reverse || true
            }"
            :ui="{
              title: 'text-center',
              description: 'text-sm text-center'
            }"
          >
            <NuxtImg
              v-if="image"
              :src="image.src"
              :alt="image.alt"
              class="w-full size-full"
            />
            <ImagePlaceholder v-else />

            <template #description>
              <div class="grid gap-5">
                <p>{{ item.description }}</p>
                <div class="flex items-center justify-center gap-4">
                  <UButton
                    icon="i-lucide-mail"
                    :to="item.social.mail"
                    target="_blank"
                    color="neutral"
                    variant="subtle"
                  />
                  <UButton
                    icon="i-lucide-instagram"
                    :to="item.social.instagram"
                    target="_blank"
                    color="neutral"
                    variant="subtle"
                  />
                  <UButton
                    icon="i-simple-icons-x"
                    :to="item.social.x"
                    color="neutral"
                    variant="subtle"
                  />
                  <UButton
                    icon="i-lucide-linkedin"
                    :to="item.social.linkedin"
                    target="_blank"
                    color="neutral"
                    variant="subtle"
                  />
                </div>
              </div>
            </template>
          </UPageCard>
        </UPageGrid>
      </MotionSection>

      <UPageSection
        id="testimonials"
        :headline="page.testimonials.headline"
        :title="page.testimonials.title"
        :description="page.testimonials.description"
      >
        <UMarquee
          pause-on-hover
          :repeat="10"
          :overlay="false"
          :ui="{ root: '[--gap:--spacing(4)]', content: 'w-auto py-1' }"
        >
          <UPageCard
            v-for="(testimonial, index) in page.testimonials.items"
            :key="index"
            :overlay="false"
            variant="subtle"
            :description="testimonial.quote"
            :ui="{ description: 'before:content-[open-quote] after:content-[close-quote]' }"
            class="w-64 shrink-0"
          >
            <template #footer>
              <UUser
                v-bind="testimonial.user"
                size="lg"
              />
            </template>
          </UPageCard>
        </UMarquee>
        <!-- <UPageColumns class="xl:columns-4">
          <UPageCard
            v-for="(testimonial, index) in page.testimonials.items"
            :key="index"
            variant="subtle"
            :description="testimonial.quote"
            :ui="{ description: 'before:content-[open-quote] after:content-[close-quote]' }"
          >
            <template #footer>
              <UUser
                v-bind="testimonial.user"
                size="lg"
              />
            </template>
          </UPageCard>
        </UPageColumns> -->
      </UPageSection>

      <USeparator />

      <UPageCTA
        v-bind="page.cta"
        variant="soft"
        class="overflow-hidden"
      >
      </UPageCTA>

      <ContactSection v-bind="page.contact" />
    </div>

    <div v-else>
      loading
    </div>
  </div>
</template>