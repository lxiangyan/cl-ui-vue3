<template>
    <header class="cl-header">
        <div class="cl-header-container flex-row">
            <div class="cl-header-container-left" @click="onLeftClick">
                <slot name="left">
                    <span>左</span>
                </slot>
            </div>
            <div class="cl-header-container-mid" @click="onCenterClick">
                <slot name="center">
                    {{ title }}
                </slot>
            </div>
            <div class="cl-header-container-right">
                <slot name="right">
                    <div class="cl-header-container-right-text" @click="onRightClick">
                        {{ rightTitle }}
                    </div>
                </slot>
            </div>
        </div>
    </header>
</template>

<script setup lang="ts">
    defineProps({
        title: String,
        rightTitle: String
    });

    const emit = defineEmits(['leftClick', 'centerClick', 'rightClick']);
    const onLeftClick = () => {
        console.log('left click');
        emit('leftClick');
    };
    const onCenterClick = () => {
        console.log('center click');
        emit('centerClick');
    };
    const onRightClick = () => {
        console.log('right click');
        emit('rightClick');
    }
</script>

<style scoped>
.flex-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
}
.cl-header {
    height: 44px;
    background: #f05a23;
}
.cl-header-container {
    height: 100%;
}
</style>