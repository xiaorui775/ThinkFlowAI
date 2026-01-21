<script setup lang="ts">
/**
 * 顶部导航栏
 * - 桌面端：展示完整工具条（视图控制/布局/配色/导出/设置等）
 * - 移动端：收纳为下拉面板，通过 callAndClose 统一“执行 + 收起”
 */

// 基础依赖
import { ref } from 'vue'

// 背景样式枚举（用于切换 Lines/Dots）
import { BackgroundVariant } from '@vue-flow/background'

// 图标：所有按钮与状态展示
import {
    ChevronDown,
    ChevronUp,
    Download,
    Focus,
    Globe,
    LayoutDashboard,
    Map,
    Menu,
    Palette,
    Settings,
    Sparkles,
    Target,
    Trash2,
    X
} from 'lucide-vue-next'

/**
 * props：
 * - t：i18n 翻译函数
 * - locale：当前语言标识（用于显示 EN/ZH）
 * - config：全局画布配置（边颜色/背景/小地图开关等）
 * - onXxx：由 App 传入的动作回调
 */
const props = defineProps<{
    t: any
    locale: string
    config: any
    onFit: () => void
    onResetLayout: () => void
    onCenterRoot: () => void
    onStartNewSession: () => void
    onGenerateSummary: () => void
    onExportMarkdown: () => void
    onOpenSettings: () => void
}>()

const emit = defineEmits<{
    (e: 'toggle-locale'): void
}>()

/**
 * 移动端工具面板是否展开
 */
const isToolsExpanded = ref(false)

/**
 * 执行某个工具动作后自动收起移动端面板
 */
const callAndClose = (fn: () => void) => {
    fn()
    isToolsExpanded.value = false
}
</script>

<template>
    <nav class="flex-none bg-white/80 backdrop-blur-md border-b border-slate-200 px-3 md:px-6 py-2 md:py-3 flex items-center justify-between shadow-sm z-50">
        <div class="flex items-center gap-2 md:gap-6 flex-grow mr-2">
            <div class="flex items-center gap-2 flex-shrink-0">
                <div class="w-3 h-3 bg-orange-500 rounded-sm rotate-45"></div>
                <span class="font-black text-slate-900 tracking-tighter text-base md:text-lg">ThinkFlow</span>
            </div>

            <div class="h-6 w-[1px] bg-slate-200 mx-1 md:mx-2 flex-shrink-0"></div>

            <div class="hidden md:flex items-center gap-2">
                <button @click="props.onFit" class="toolbar-btn text-blue-500 hover:bg-blue-50 border-blue-100 flex-shrink-0" :title="props.t('nav.fit')">
                    <Focus class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.fit') }}</span>
                </button>

                <button @click="props.onResetLayout" class="toolbar-btn text-purple-500 hover:bg-purple-50 border-purple-100 flex-shrink-0" :title="props.t('nav.layout')">
                    <LayoutDashboard class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.layout') }}</span>
                </button>

                <button @click="props.onCenterRoot" class="toolbar-btn text-orange-500 hover:bg-orange-50 border-orange-100 flex-shrink-0" :title="props.t('nav.center')">
                    <Target class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.center') }}</span>
                </button>

                <div class="h-4 w-[1px] bg-slate-100 mx-1 flex-shrink-0"></div>

                <button @click="props.onStartNewSession" class="toolbar-btn text-red-500 hover:bg-red-50 border-red-100 flex-shrink-0" :title="props.t('nav.reset')">
                    <Trash2 class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.reset') }}</span>
                </button>

                <div class="h-4 w-[1px] bg-slate-100 mx-1 flex-shrink-0"></div>

                <div class="flex items-center gap-2 px-2 md:px-3 py-1.5 bg-slate-50 rounded-lg border border-slate-100 flex-shrink-0">
                    <Palette class="w-3 h-3 md:w-3.5 h-3.5 text-slate-400" />
                    <input type="color" v-model="props.config.edgeColor" class="w-3.5 h-3.5 md:w-4 h-4 rounded cursor-pointer bg-transparent border-none" />
                    <span class="text-[9px] md:text-[10px] font-bold text-slate-500 uppercase">{{ props.t('nav.edge') }}</span>
                </div>

                <select v-model="props.config.backgroundVariant" class="toolbar-select flex-shrink-0">
                    <option :value="BackgroundVariant.Lines">{{ props.t('nav.lines') }}</option>
                    <option :value="BackgroundVariant.Dots">{{ props.t('nav.dots') }}</option>
                </select>

                <div class="h-4 w-[1px] bg-slate-100 mx-1 flex-shrink-0"></div>

                <button
                    @click="props.config.showMiniMap = !props.config.showMiniMap"
                    class="toolbar-btn border-slate-100 flex-shrink-0"
                    :class="props.config.showMiniMap ? 'text-blue-500 bg-blue-50 border-blue-100' : 'text-slate-400 hover:text-slate-600'"
                    :title="props.t('nav.map')"
                >
                    <Map class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.map') }}</span>
                </button>

                <div class="h-4 w-[1px] bg-slate-100 mx-1 flex-shrink-0"></div>

                <button @click="props.onGenerateSummary" class="toolbar-btn text-orange-600 hover:bg-orange-50 border-orange-100 flex-shrink-0" :title="props.t('nav.summary')">
                    <Sparkles class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.summary') }}</span>
                </button>

                <button @click="props.onExportMarkdown" class="toolbar-btn text-indigo-600 hover:bg-indigo-50 border-indigo-100 flex-shrink-0" :title="props.t('nav.export')">
                    <Download class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('nav.export') }}</span>
                </button>

                <div class="h-4 w-[1px] bg-slate-100 mx-1 flex-shrink-0"></div>

                <button @click="props.onOpenSettings" class="toolbar-btn text-slate-600 hover:bg-slate-50 border-slate-100 flex-shrink-0">
                    <Settings class="w-3.5 h-3.5 md:w-4 h-4" />
                    <span>{{ props.t('common.settings') }}</span>
                </button>
            </div>

            <div class="md:hidden flex items-center">
                <button
                    @click="isToolsExpanded = !isToolsExpanded"
                    class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-lg text-slate-600 active:bg-slate-100 transition-colors"
                >
                    <Menu v-if="!isToolsExpanded" class="w-4 h-4" />
                    <X v-else class="w-4 h-4" />
                    <span class="text-xs font-bold">{{ props.t('common.tools') || 'Tools' }}</span>
                    <ChevronDown v-if="!isToolsExpanded" class="w-3 h-3 opacity-50" />
                    <ChevronUp v-else class="w-3 h-3 opacity-50" />
                </button>
            </div>
        </div>

        <div class="flex items-center gap-2 md:gap-3 flex-shrink-0">
            <button
                @click="emit('toggle-locale')"
                class="p-1.5 md:p-2 hover:bg-slate-100 rounded-md transition-colors text-slate-400 font-bold text-[10px] md:text-xs flex items-center gap-1"
            >
                <Globe class="w-3 h-3 md:w-3.5 h-3.5" /> {{ props.locale === 'zh' ? 'EN' : 'ZH' }}
            </button>
        </div>
    </nav>

    <Transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="transform -translate-y-4 opacity-0"
        enter-to-class="transform translate-y-0 opacity-100"
        leave-active-class="transition duration-150 ease-in"
        leave-from-class="transform translate-y-0 opacity-100"
        leave-to-class="transform -translate-y-4 opacity-0"
    >
        <div v-if="isToolsExpanded" class="md:hidden absolute top-[57px] left-0 right-0 bg-white/95 backdrop-blur-md border-b border-slate-200 shadow-xl z-40 py-4 px-4 flex flex-wrap gap-3 justify-center">
            <button @click="callAndClose(props.onFit)" class="toolbar-btn text-blue-500 hover:bg-blue-50 border-blue-100" :title="props.t('nav.fit')">
                <Focus class="w-4 h-4" />
                <span>{{ props.t('nav.fit') }}</span>
            </button>

            <button @click="callAndClose(props.onResetLayout)" class="toolbar-btn text-purple-500 hover:bg-purple-50 border-purple-100" :title="props.t('nav.layout')">
                <LayoutDashboard class="w-4 h-4" />
                <span>{{ props.t('nav.layout') }}</span>
            </button>

            <button @click="callAndClose(props.onCenterRoot)" class="toolbar-btn text-orange-500 hover:bg-orange-50 border-orange-100" :title="props.t('nav.center')">
                <Target class="w-4 h-4" />
                <span>{{ props.t('nav.center') }}</span>
            </button>

            <button @click="callAndClose(props.onStartNewSession)" class="toolbar-btn text-red-500 hover:bg-red-50 border-red-100" :title="props.t('nav.reset')">
                <Trash2 class="w-4 h-4" />
                <span>{{ props.t('nav.reset') }}</span>
            </button>

            <div class="flex items-center gap-2 px-3 py-1.5 bg-slate-50 rounded-lg border border-slate-100">
                <Palette class="w-4 h-4 text-slate-400" />
                <input type="color" v-model="props.config.edgeColor" class="w-4 h-4 rounded cursor-pointer bg-transparent border-none" />
                <span class="text-[10px] font-bold text-slate-500 uppercase">{{ props.t('nav.edge') }}</span>
            </div>

            <select v-model="props.config.backgroundVariant" class="toolbar-select">
                <option :value="BackgroundVariant.Lines">{{ props.t('nav.lines') }}</option>
                <option :value="BackgroundVariant.Dots">{{ props.t('nav.dots') }}</option>
            </select>

            <button
                @click="props.config.showMiniMap = !props.config.showMiniMap"
                class="toolbar-btn border-slate-100"
                :class="props.config.showMiniMap ? 'text-blue-500 bg-blue-50 border-blue-100' : 'text-slate-400 hover:text-slate-600'"
                :title="props.t('nav.map')"
            >
                <Map class="w-4 h-4" />
                <span>{{ props.t('nav.map') }}</span>
            </button>

            <button @click="callAndClose(props.onGenerateSummary)" class="toolbar-btn text-orange-600 hover:bg-orange-50 border-orange-100" :title="props.t('nav.summary')">
                <Sparkles class="w-4 h-4" />
                <span>{{ props.t('nav.summary') }}</span>
            </button>

            <button @click="callAndClose(props.onExportMarkdown)" class="toolbar-btn text-indigo-600 hover:bg-indigo-50 border-indigo-100" :title="props.t('nav.export')">
                <Download class="w-4 h-4" />
                <span>{{ props.t('nav.export') }}</span>
            </button>

            <button @click="callAndClose(props.onOpenSettings)" class="toolbar-btn text-slate-600 hover:bg-slate-50 border-slate-100">
                <Settings class="w-4 h-4" />
                <span>{{ props.t('common.settings') }}</span>
            </button>
        </div>
    </Transition>
</template>
