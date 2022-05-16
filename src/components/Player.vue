<template>
  <video ref="videoRef" controls autoplay className="h-full w-full"></video>
</template>

<script lang="ts" setup>
import { ElMessage } from 'element-plus'
import Hls from 'hls.js'
import { ref, onMounted, watch } from 'vue'

const videoRef = ref<HTMLVideoElement>(null!)
const props = defineProps<{ m3u8Url: string }>()

watch(
  () => props.m3u8Url,
  () => {
    loadM3u8Video()
  }
)

onMounted(() => {
  loadM3u8Video()
})

const loadM3u8Video = () => {
  if (Hls.isSupported()) {
    const hls = new Hls({ autoStartLoad: true })
    if (hls) {
      hls.attachMedia(videoRef.value)
      hls.on(Hls.Events.MEDIA_ATTACHED, function () {
        console.log('video and hls.js are now bound together !')
        hls.loadSource(props.m3u8Url)
        hls.on(Hls.Events.MANIFEST_PARSED, function (event, data) {
          console.log(
            'manifest loaded, found ' + data.levels.length + ' quality level'
          )
        })
      })
      hls.on(Hls.Events.ERROR, function (event, data) {
        const errorType = data.type
        console.error(event, data)
        ElMessage.warning(`hls on ${errorType}`)
        hls.destroy()
      })
      // hls.on(Hls.Events.MANIFEST_PARSED, function () {
      //     videoRef.value.play()
      //   })
    } else if (videoRef.value.canPlayType('application/vnd.apple.mpegurl')) {
      videoRef.value.src = props.m3u8Url
      videoRef.value.addEventListener('loadedmetadata', function () {
        videoRef.value.play()
      })
    }
  } else {
    ElMessage.warning('hls is not supported!')
  }
}
</script>

<style scoped></style>
