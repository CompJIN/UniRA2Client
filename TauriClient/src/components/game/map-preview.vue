<script lang="ts" setup>
import { onMounted, onUpdated, PropType, reactive } from 'vue';
import { GameMap } from '@/game/map';

const props = defineProps({
  map: {
    type: Object as PropType<GameMap>,
    required: true,
  },
});


const preview = reactive({
  ready: false,
  value: '',
})

const loadMap = async () => {
  const map = props.map
  if (!map) {
    return;
  }
  preview.value = await map.cover;
  console.log('map', preview.value)
  preview.ready = true;
}

onMounted(loadMap)
onUpdated(loadMap)
</script>

<template>
  <div class="map-preview-root">
    <!-- <img class="preview-border" src="/game/map_preview_box.png" draggable="false" /> -->
    <img class="map-preview" :src="preview.value" draggable="false" />
    <!-- <div class="desc">
      <div class="en">{{ map.name.en }}</div>
      <div class="zh">{{ map.name.zh }}</div>
    </div> -->
    <!-- <div class="player-count-box">
      <div class="extra-gradient flex">
        <div class="count">{{ map.playerLimit[0] }} - {{ map.playerLimit[1] }}</div>
        <div>Players</div>
      </div>
    </div> -->
  </div>
</template>

<style scoped>
.map-preview-root {
  width: 100%;
}

.preview-border {
  width: 100%;
  object-fit: contain;
  z-index: 10;
  
}

.map-preview {
  top: 0;
  object-fit: contain;
  left: 0;
  width: 100%;
}

.desc {
  position: absolute;
  right: 30px;
  bottom: 24px;
  text-align: right;
  --shadow-blur-color: 2px rgba(0, 0, 0, 0.8);
  text-shadow: 2px 0 var(--shadow-blur-color), -2px 0 var(--shadow-blur-color), 0 var(--shadow-blur-color),
    0 -2px var(--shadow-blur-color);
  color: rgb(153, 188, 235);
}

.desc>.en {
  font-size: 46px;
}

.desc>.zh {
  font-size: 24px;
  font-family: '汉仪细等线';
  font-weight: bolder;
}

.player-count-box {
  position: absolute;
  width: 40%;
  left: 16px;
  top: 12px;
  background-image: linear-gradient(to right, rgba(3, 98, 161, 0.48) 0 35%, transparent);
  padding: 2px 0;
}

.player-count-box>.extra-gradient {
  width: 100%;
  padding: 4px;
  background-image: linear-gradient(to right, rgba(3, 98, 161, 0.68) 0 35%, transparent);
  align-items: flex-end;
}

.player-count-box div {
  font-family: BureauAgency;
  font-size: 20px;
}

.player-count-box .count {
  font-size: 40px;
  margin-right: 4px;
}
</style>
