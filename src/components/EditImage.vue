<template>
  <div class="image_container">
    <img ref="imageRef" :src="imageUrl" @load="init">
  </div>
  <div class="action">
    <el-button >裁剪</el-button>
    <el-button>60X60</el-button>
    <span>宽</span>
    <el-input v-model.number="cropperWidth" style="width: 120px" @change="setWidthHeight"></el-input>
    <span>高</span>
    <el-input v-model.number="cropperHeight" style="width: 120px" @change="setWidthHeight"></el-input>
  </div>
</template>


<script setup lang="ts">
import Cropper from "cropperjs";
import 'cropperjs/dist/cropper.css';
import {onMounted, reactive, toRefs, ref} from "vue";

interface Props {
  imageUrl: string,
}

const props = defineProps<Props>();

interface State {
  cropperInstance: unknown,
  imageUrl: string,
  cropperWidth: number,
  cropperHeight: number,
}

const state:State = reactive({
  cropperInstance: null,
  imageUrl: '',
  cropperWidth: 0,
  cropperHeight: 0,
});

const {cropperInstance,imageUrl, cropperWidth, cropperHeight} = toRefs(state);

const imageRef = ref<HTMLImageElement>();

onMounted(() => {
  imageUrl.value = props.imageUrl;
})

const options = {
  crop(event:CustomEvent) {
    console.log(event)
    cropperWidth.value = event.detail.width;
    cropperHeight.value = event.detail.height;
  }
}

const init = () => {
  cropperInstance.value = new Cropper(imageRef.value, options);
}
</script>


<style lang="scss" scoped>
.image_container {
  width: 300px;
  height: 300px;

  img {
    display: block;
    max-width: 100%;
  }
}

.action {
  margin: 20px;
}
</style>


