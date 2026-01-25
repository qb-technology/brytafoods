<script setup lang="ts">
import { reactify, useMediaControls } from '@vueuse/core';
import { computed, reactive, shallowRef, useTemplateRef } from 'vue';

const video = useTemplateRef('video');
const loop = shallowRef(false);
const poster = 'https://r2.qbtech.dev/brytafoods/advertisements.jpg';

const controls = useMediaControls(video, {
  src: {
    src: 'https://r2.qbtech.dev/brytafoods/investors_page_brytafoods.mp4',
    type: 'video/mp4',
  },
});

const {
  playing,
  buffered,
  currentTime,
  duration,
  tracks,
  waiting,
  selectedTrack,
  volume,
  muted,
  isPictureInPicture,
  supportsPictureInPicture,
  togglePictureInPicture,
  enableTrack,
  disableTrack,
} = controls;
const endBuffer = computed(() => {
  const ranges = buffered.value;
  if (!ranges || ranges.length === 0) return 0;

  return ranges[ranges.length - 1]?.[1] ?? 0;
});
function formatDuration(seconds: number) {
  return new Date(1000 * seconds).toISOString().slice(14, 19);
}
</script>

<template>
  <div
    class="outline-none"
    :tabindex="0"
    autofocus
    @keydown.prevent.space="playing = !playing"
    @keydown.right="currentTime += 10"
    @keydown.left="currentTime -= 10"
  >
    <div
      class="mt-5 relative bg-black rounded-md shadow overflow-hidden"
    >
      <video
        ref="video"
        class="w-full block"
        :poster="poster"
        :loop="loop"
        @click="playing = !playing"
      />
      <div
        v-if="waiting"
        class="absolute inset-0 grid place-items-center pointer-events-none bg-black bg-opacity-20 w-full aspect-video"
      >
        <VideoSpinner />
      </div>
    </div>

    <div class="flex items-center gap-1">
      <UButton
        color="neutral"
        variant="ghost"
        :icon="playing ? 'i-lucide-pause' : 'i-lucide-play'"
        @click="() => { playing = !playing }"
      />
      <VideoScrubber
        v-model="currentTime"
        :max="duration"
        :secondary="endBuffer"
        class="mt-2 flex-1"
      >
        <template #default="{ position, pendingValue }">
          <div
            class="absolute transform -translate-x-1/2 bg-black rounded px-2 bottom-0 mb-4 py-1 text-xs text-white"
            :style="{ left: position }"
          >
            {{ formatDuration(pendingValue) }}
          </div>
        </template>
      </VideoScrubber>
    </div>

    <div class="flex flex-row items-center">
      <UButton
        color="neutral"
        variant="ghost"
        :icon="muted ? 'i-lucide-volume-x' : 'i-lucide-volume-2'"
        @click="() => { muted = !muted }"
      />

      <VideoScrubber
        v-model="volume"
        :max="1"
        class="w-32 ml-2"
      />
      <div class="flex flex-col flex-1 text-sm ml-2">
        {{ formatDuration(currentTime) }} / {{ formatDuration(duration) }}
      </div>

      <VideoMenu class="mr-2">
        <!-- <template #default="{ open }">
          <UButton
            color="neutral"
            variant="ghost"
            icon="i-lucide:closed-caption"
            @click="() => { open() }"
          />
        </template> -->
        <template #menu="{ close }">
          <div class="absolute bottom-0 right-0 bg-black rounded py-2 shadow">
            <VideoMenuItem
              @keydown.stop.prevent.enter.space="disableTrack()"
              @click="
                () => {
                  disableTrack();
                  close();
                }
              "
            >
              <span class="flex-1">Off</span>
              <UIcon
                :class="{ 'opacity-0': selectedTrack !== -1 }"
                name="i-lucide-check"
              />
            </VideoMenuItem>
            <VideoMenuItem
              v-for="track in tracks"
              :key="track.id"
              @keydown.stop.prevent.enter.space="enableTrack(track)"
              @click="
                () => {
                  enableTrack(track);
                  close();
                }
              "
            >
              <span class="flex-1">{{ track.label }}</span>
              <UIcon
                :class="{ 'opacity-0': track.mode !== 'showing' }"
                name="i-lucide-check"
              />
            </VideoMenuItem>
          </div>
        </template>
      </VideoMenu>
      <VideoMenu class="mr-2">
        <template #default="{ open }">
          <UButton
            variant="ghost"
            color="neutral"
            icon="i-lucide-settings"
            @click="() => { open() }"
          />
        </template>
        <template #menu="{ close }">
          <div class="absolute bottom-0 right-0 shadow py-2 bg-black rounded">
            <VideoMenuItem
              v-if="supportsPictureInPicture"
              @click="
                () => {
                  togglePictureInPicture();
                  close();
                }
              "
            >
              <i i-carbon-popup />
              <span>
                {{ isPictureInPicture ? 'Exit' : 'Enter' }} Picture in Picture
              </span>
            </VideoMenuItem>
            <VideoMenuItem
              @click="
                () => {
                  loop = !loop;
                  close();
                }
              "
            >
              <i i-carbon-repeat />
              <span class="flex-1">Loop</span>
              <i
                v-if="loop"
                i-carbon-checkmark
              />
            </VideoMenuItem>
          </div>
        </template>
      </VideoMenu>
      <VideoMenu>
        <template #default="{ open }">
          <button
            class="block"
            @click="open()"
          >
            <i
              i-carbon-meter
              inline-block
              align-middle
            />
          </button>
        </template>
        <template #menu="{ close }">
          <div class="absolute bottom-0 right-0 shadow py-2 bg-black rounded">
            <VideoMenuItem
              @click="
                () => {
                  controls.rate.value = 2;
                  close();
                }
              "
            >
              <i i-carbon-meter-alt />
              2x
            </VideoMenuItem>
            <VideoMenuItem
              @click="
                () => {
                  controls.rate.value = 1;
                  close();
                }
              "
            >
              <i i-carbon-meter-alt />
              1x
            </VideoMenuItem>
          </div>
        </template>
      </VideoMenu>
    </div>
  </div>
</template>
