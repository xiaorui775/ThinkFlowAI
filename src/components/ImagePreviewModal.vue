<script setup lang="ts">
/**
 * 图片预览弹窗
 * - 接收 url，存在则显示
 * - 点击遮罩或关闭按钮触发 close
 */

// 图标：关闭按钮
import { X } from 'lucide-vue-next'

/**
 * props：
 * - url：需要预览的图片地址（null 表示关闭）
 */
const props = defineProps<{
    url: string | null
}>()

/**
 * close：关闭预览
 */
const emit = defineEmits<{
    (e: 'close'): void
}>()
</script>

<template>
    <Transition name="fade">
        <div v-if="props.url" class="fixed inset-0 z-[100] flex items-center justify-center bg-black/80 backdrop-blur-sm p-10" @click="emit('close')">
            <div class="relative max-w-full max-h-full rounded-lg overflow-hidden shadow-2xl animate-in fade-in zoom-in duration-300" @click.stop>
                <button @click="emit('close')" class="absolute top-4 right-4 p-2 bg-black/50 hover:bg-black/70 text-white rounded-full transition-colors z-10">
                    <X class="w-5 h-5" />
                </button>
                <img :src="props.url" class="max-w-screen max-h-screen object-contain" />
            </div>
        </div>
    </Transition>
</template>
