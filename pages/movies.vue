<script setup lang="ts">
import { parse, type HTMLElement } from 'node-html-parser'
import { Carousel, Slide } from 'vue3-carousel'
import 'vue3-carousel/dist/carousel.css'

type Film = {
  title: string
  id: string
  image: string
}

const { data: nowplaingFetch } = useFetch<string>('https://uecmovies.com/movies/nowplaying')
const { data: comingsoonFetch } = useFetch<string>('https://uecmovies.com/movies/comingsoon')
const { data: specialeventsFetch } = useFetch<string>('https://uecmovies.com/movies/specialevents')

const nowplaying = computed(() => {
  if (!nowplaingFetch.value) return null

  return extractFilmData(parse(nowplaingFetch.value))
})
const comingsoon = computed(() => {
  if (!comingsoonFetch.value) return null

  return extractFilmData(parse(comingsoonFetch.value))
})
const specialevents = computed(() => {
  if (!specialeventsFetch.value) return null

  return extractFilmData(parse(specialeventsFetch.value))
})

function extractFilmData(element: HTMLElement): Film[] {
  return element
    .querySelectorAll('a')
    .map((el) => {
      const img = el.querySelector('img')
      if (!img) return
      return {
        title: el.getAttribute('title') as string,
        id: el.getAttribute('href')?.split('/').at(-1) as string,
        image: `//uecmovies.com${img.getAttribute('src')}`,
      }
    })
    .filter((v) => !!v)
}

function film(f?: Film) {
  if (!f) return h('div', { class: '' }, [h('div', { class: 'h-60 bg-white block aspect-9/16' })])
  return h('div', { class: 'min-w-52' }, [h('img', { src: f.image, class: 'w-52', draggable: 'false' })])
}

defineComponent(() => {})

useHead({
  title: 'Movies - UEC Movies',
})
</script>

<template>
  <div class="space-y-4">
    <div>
      <div class="max-w-5xl mx-auto w-screen">
        <h2 class="font-bold text-4xl py-6">NOW PLAYING</h2>
      </div>

      <div>
        <carousel :items-to-show="1.5">
          <slide
            v-for="slide in 10"
            :key="slide"
          >
            {{ slide }}
          </slide>
        </carousel>

        <u-carousel class="max-w-5xl mx-auto flex gap-4">
          <film
            v-for="f of nowplaying"
            v-bind="f"
          />
        </u-carousel>
      </div>
    </div>
    <div></div>
    <div></div>
  </div>
</template>
