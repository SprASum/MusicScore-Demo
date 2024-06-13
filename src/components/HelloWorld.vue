<template>
  <div class="container">
    <button @click="play">测试播放</button>
    <div class="fileUpload">
      <input type="file" @change="loadFile">
    </div>
    <div class="at-viewport" ref="atViewport">
      <div ref="alphaTab"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {onMounted, ref} from "vue"
import {AlphaTabApi} from "@coderline/alphatab"

const alphaTab = ref<HTMLElement | null>(null)
const api = ref<AlphaTabApi | null>(null)

const atViewport = ref<HTMLElement | null>(null)

function loadFile(event: Event) {
  const file = (event.target as HTMLInputElement).files![0]

  if (file && api.value) {
    alphaTab.value!.innerHTML = ""

    api.value = new AlphaTabApi(alphaTab.value!, {
      file: "../src/GuitarProProject/" + file.name,
      core: {
        fontDirectory: '/font/'
      },
      player: {
        enablePlayer: true,
        soundFont: "https://cdn.jsdelivr.net/npm/@coderline/alphatab@latest/dist/soundfont/sonivox.sf2",
        scrollElement: atViewport.value
      },
    })
  }
}

function play() {
  api.value?.play()
}

onMounted(() => {
  if (alphaTab.value) {
    api.value = new AlphaTabApi(alphaTab.value, {
      file: "https://www.alphatab.net/files/canon.gp",
      core: {
        fontDirectory: '/font/'
      },
      player: {
        enablePlayer: true,
        soundFont: "https://cdn.jsdelivr.net/npm/@coderline/alphatab@latest/dist/soundfont/sonivox.sf2",
        scrollElement: atViewport.value
      },
    })
    console.log("init AlphaTabApi...")
  } else {
    console.error("AlphaTab element not found")
  }
})
</script>

<style lang="scss" scoped>
.at-viewport {
  overflow-y: auto;
  position: absolute;
  top: 0;
  left: 70px;
  right: 0;
  bottom: 0;
  padding-right: 20px;
}

:deep(.at-cursor-bar) {
  /* Defines the color of the bar background when a bar is played */
  background: rgba(255, 242, 0, 0.25);
}

:deep(.at-selection div) {
  /* Defines the color of the selection background */
  background: rgba(64, 64, 255, 0.1);
}

:deep(.at-cursor-beat) {
  /* Defines the beat cursor */
  background: rgba(64, 64, 255, 0.75);
  width: 3px;
}

:deep(.at-highlight *) {
  /* Defines the color of the music symbols when they are being played (svg) */
  fill: #0078ff;
  stroke: #0078ff;
}
</style>
