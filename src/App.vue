<script setup lang="ts">
import { Model, World, Camera, Group } from "lingo3d-vue"
import { settings } from "lingo3d"
import { mapRange } from "@lincode/math"
import { ref, onMounted, computed } from "vue"
import { useWindowSize } from "vue-window-size"

settings.defaultLight = false

const scrollY = ref(0)
const screenAngle = computed(() => mapRange(scrollY.value, 0, 500, 110, 0, true))
const angle = computed(() => mapRange(scrollY.value, 0, 500, 0, 30))
const z = computed(() => mapRange(scrollY.value, 0, 500, 100, 200, true))

const { width, height } = useWindowSize()
const fov = width.value > height.value ? 75 : 100

onMounted(() => {
  document.addEventListener("scroll", (e) => {
    scrollY.value = window.scrollY
  })
})
</script>

<template>
  <div class="w-full text-[50px] text-center mt-[50vh]">MacBook Pro</div>
  <World defaultLight="studio" position="fixed" color="transparent">
    <Group :rotationY="angle">
      <Model src="body.glb" :boxVisible="false" />
      <Model src="screen.glb" :y="-45" :z="-35" :innerY="47" :innerZ="-20" :rotationX="screenAngle" />
    </Group>
    <Camera :z="z" active :fov="fov" />
  </World>
</template>
