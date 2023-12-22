<script setup lang="ts">
import { onDeactivated, ref, watch } from 'vue';

const props = defineProps({
    // 是否显示
    modelValue: {
        type: Boolean,
        default: false
    },
    // 开启transfer使组件放入body下
    teleport: {
        type: [String, Element],
        default: 'body'
    },
    // 开启transfer后组件挂载的父元素
    transfer: {
        type: Boolean,
        default: true
    },
    // 透明背景
    isTransparent: {
        type: Boolean,
        default: false
    },
    // 是否显示遮罩层
    showMask: {
        type: Boolean,
        default: true
    },
    // 点击遮罩层是否自动关闭
    hideOnMaskClick: {
        type: Boolean,
        default: true
    },
    // deactivated钩子回调自动关闭
    hideOnDeactivated: {
        type: Boolean,
        default: true
    },
    // 关闭弹窗前的钩子，直接调用modelValue=false不会触发beforeClose，必需通过hide()触发
    beforeClose: {
        type: Function,
        default: null
    }
});
const emit = defineEmits(['show', 'hide', 'update:modelValue']);
const visible = ref(props.modelValue);
watch(
    () => props.modelValue,
    (newValue) => {
        visible.value = newValue;
    }
);
watch(
    visible,
    (newValue) => {
        emit(newValue ? 'show' : 'hide');
    }
)
const doHide = () => {
    visible.value = false;
    emit('update:modelValue', false);
};
const hide = () => {
    if(typeof props.beforeClose === 'function') {
        props.beforeClose(() => {
            doHide();
        });
    } else {
        doHide();
    }
};
const onMaskClick = () => {
    props.hideOnMaskClick && hide();
};
const onTounchmove = (event: Event) => {
    event.preventDefault();
};
onDeactivated(() => {
    props.hideOnDeactivated && doHide();
})
</script>

<template>
    <Teleport :disabled="!transfer" :to="teleport">
        <Transition>
            <div v-show="visible">
                <div v-show="showMask" 
                    :class="['cl-popup-mask', isTransparent ? 'is-transparent' : '']" 
                    @click="onMaskClick" 
                    @touchmove="onTounchmove"></div>
                <slot></slot>
            </div>
        </Transition>
    </Teleport>
</template>