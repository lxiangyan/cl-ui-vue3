<script setup lang="ts">
import { ref } from 'vue';
import clPopup from '@/components/popup/index.vue';

const popupValue = ref(false)
const onBtnClick = () => {
    popupValue.value = true;
}
const onBtnCloseClick = () => {
    popupValue.value = false;
}
</script>

<template>
    <div>
        <button @click="onBtnClick">popup 遮罩层</button>
        <clPopup v-model="popupValue">
            <div>popup 内容</div>
            <button @click="onBtnCloseClick">关闭</button>
        </clPopup>
    </div>
</template>