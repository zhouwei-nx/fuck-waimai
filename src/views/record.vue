<script setup lang="ts">
import { onMounted, ref } from 'vue';
onMounted(() => {
})
import screenfull from 'screenfull'

const handleFullScreen = () => {
    if (!screenfull.isFullscreen) {
        screenfull.toggle()
        screenfull.request()
    }
}
const assetsImg = (filename: string) => new URL(`/src/assets/images/${filename}`, import.meta.url).href;

const imgContent = ref(assetsImg('screen.jpg'))
const afterRead = (file: any) => {
    // 此时可以自行将文件上传至服务器
    imgContent.value = file.objectUrl
};
const show = ref(false)
const sheetTollage = () => {
    handleFullScreen()
    show.value = !show.value
}
const slideValue = ref(15);
const windowHeight = window.innerHeight
const cropHeight = ref(windowHeight * 0.15)
const onChange = (value: number) => {
    cropHeight.value = windowHeight * value * 0.01
}
</script>
<template>
    <div class="container">
        <van-image class="top" fit="cover" position="top" width="100%" :style="{ height: cropHeight + 'px' }"
            :src="imgContent" @click="sheetTollage" />
        <van-image width="100%" :src="imgContent" @click="sheetTollage"></van-image>
        <van-action-sheet v-model:show="show" title="参数配置">
            <div class="sheet">
                <van-field name="slider" label="顶部图片高度" label-align="top">
                    <template #input>
                        <van-slider class="slider" v-model="slideValue" bar-height="6px" active-color="#ee0a24"
                            @change="onChange" />
                    </template>
                </van-field>
                <van-field name="slider" label="长截图" label-align="top">
                    <template #input>
                        <van-uploader :after-read="afterRead" />
                    </template>
                </van-field>
            </div>
        </van-action-sheet>
    </div>
</template>
<style scoped>
.container {
    width: 100vw;
    height: 100vh;
    overflow: scroll;
    overflow-y: auto;
    position: relative;

}

.top {
    position: sticky;
    top: 0;
    width: 100%;
    min-height: 10%;
    z-index: 1;
}

.sheet {
    padding: 16px 16px 160px;
}
</style>
