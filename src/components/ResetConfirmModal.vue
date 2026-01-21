<script setup lang="ts">
/**
 * 重置确认弹窗
 * - 用于“新会话/清空画布”前的二次确认
 */

// 图标：旋转提示
import { RefreshCw } from 'lucide-vue-next'

/**
 * props：
 * - show：弹窗显示开关
 * - t：i18n 翻译函数
 */
const props = defineProps<{
    show: boolean
    t: any
}>()

/**
 * 事件：
 * - close：取消/关闭
 * - confirm：确认重置（由 App/useThinkFlow 执行真正清空）
 */
const emit = defineEmits<{
    (e: 'close'): void
    (e: 'confirm'): void
}>()
</script>

<template>
    <Transition name="fade">
        <div v-if="props.show" class="fixed inset-0 z-[100] flex items-center justify-center p-4">
            <div class="absolute inset-0 bg-slate-900/40 backdrop-blur-sm" @click="emit('close')"></div>
            <div class="relative bg-white rounded-2xl shadow-2xl border border-slate-100 p-6 w-full max-w-sm overflow-hidden group animate-in zoom-in duration-300">
                <div class="absolute -top-12 -right-12 w-24 h-24 bg-orange-50 rounded-full blur-2xl group-hover:bg-orange-100 transition-colors"></div>

                <div class="relative flex flex-col items-center text-center space-y-4">
                    <div class="w-16 h-16 bg-orange-50 rounded-2xl flex items-center justify-center text-orange-500 mb-2 ring-4 ring-orange-50/50">
                        <RefreshCw class="w-8 h-8 animate-spin-slow" />
                    </div>

                    <div class="space-y-2">
                        <h3 class="text-lg font-bold text-slate-800 tracking-tight">{{ props.t('nav.reset') }}</h3>
                        <p class="text-sm text-slate-500 leading-relaxed px-4">
                            {{ props.t('common.confirmReset') }}
                        </p>
                    </div>

                    <div class="flex items-center gap-3 w-full pt-2">
                        <button
                            @click="emit('close')"
                            class="flex-1 px-4 py-2.5 rounded-xl border border-slate-200 text-slate-600 font-medium hover:bg-slate-50 transition-colors active:scale-95"
                        >
                            {{ props.t('common.cancel') || 'Cancel' }}
                        </button>
                        <button
                            @click="emit('confirm')"
                            class="flex-1 px-4 py-2.5 rounded-xl bg-orange-500 text-white font-medium hover:bg-orange-600 shadow-lg shadow-orange-500/30 transition-all active:scale-95"
                        >
                            {{ props.t('common.confirm') || 'Confirm' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </Transition>
</template>
