<script setup lang="ts">
/**
 * 总结弹窗
 * - show 控制显示
 * - isSummarizing 为 true 时展示 loading
 * - summaryContent 展示 AI 总结结果
 */

// 图标：标题/关闭/加载
import { RefreshCw, Sparkles, X } from 'lucide-vue-next'

/**
 * props：
 * - show：弹窗显示开关
 * - t：i18n 翻译函数
 * - isSummarizing：是否正在生成总结
 * - summaryContent：总结文本内容
 */
const props = defineProps<{
    show: boolean
    t: any
    isSummarizing: boolean
    summaryContent: string
}>()

/**
 * close：关闭弹窗
 */
const emit = defineEmits<{
    (e: 'close'): void
}>()
</script>

<template>
    <Transition name="fade">
        <div v-if="props.show" class="fixed inset-0 z-[100] flex items-center justify-center p-4">
            <div class="absolute inset-0 bg-slate-900/40 backdrop-blur-sm" @click="emit('close')"></div>
            <div class="relative bg-white rounded-3xl shadow-2xl border border-slate-100 w-full max-w-2xl overflow-hidden animate-in zoom-in duration-300">
                <div class="absolute top-0 left-0 right-0 h-2 bg-gradient-to-r from-orange-400 to-rose-400"></div>

                <div class="p-6 md:p-8">
                    <div class="flex items-center justify-between mb-6">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 bg-orange-50 rounded-xl flex items-center justify-center text-orange-500">
                                <Sparkles class="w-5 h-5" />
                            </div>
                            <div>
                                <h3 class="text-xl font-black text-slate-800 tracking-tight">{{ props.t('common.summaryTitle') }}</h3>
                                <p class="text-xs text-slate-400 font-medium uppercase tracking-wider">{{ props.t('common.aiGenerated') }}</p>
                            </div>
                        </div>
                        <button @click="emit('close')" class="p-2 hover:bg-slate-50 rounded-full text-slate-400 transition-colors">
                            <X class="w-5 h-5" />
                        </button>
                    </div>

                    <div class="relative min-h-[200px] max-h-[60vh] overflow-y-auto pr-2 custom-scrollbar">
                        <div v-if="props.isSummarizing" class="absolute inset-0 flex flex-col items-center justify-center space-y-4">
                            <RefreshCw class="w-8 h-8 text-orange-500 animate-spin" />
                            <p class="text-sm font-bold text-slate-400 animate-pulse">{{ props.t('common.summarizing') }}</p>
                        </div>
                        <div v-else class="prose prose-slate max-w-none">
                            <div class="whitespace-pre-wrap text-slate-600 leading-relaxed text-sm md:text-base font-medium">
                                {{ props.summaryContent }}
                            </div>
                        </div>
                    </div>

                    <div class="mt-8 flex justify-end">
                        <button
                            @click="emit('close')"
                            class="px-8 py-3 bg-slate-900 text-white rounded-2xl text-xs font-black tracking-widest hover:bg-slate-800 transition-all shadow-xl shadow-slate-200 active:scale-95"
                        >
                            {{ props.t('common.close') }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </Transition>
</template>

