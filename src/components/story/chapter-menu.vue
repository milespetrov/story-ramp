<template>
    <div
        :class="isMenuOpen ? 'w-72' : ''"
        class="nav-bar sticky self-start w-12 duration-500 ease-in-out transition-width top-16"
    >
        <div class="flex items-center mt-4 mb-12">
            <button
                class="flex items-center flex-shrink-0 px-2 py-1 mx-1 overflow-hidden"
                :aria-label="$t('chapters.menu')"
                @click="isMenuOpen = !isMenuOpen"
                v-tippy="{
                    delay: '200',
                    placement: 'right',
                    content: $t('chapters.title'),
                    onShow: () => !isMenuOpen,
                    animateFill: true
                }"
            >
                <svg
                    class="flex-shrink-0"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    stroke="#707070"
                >
                    <path class="transition-all duration-500 ease-in-out" :d="`m3.5 7h${isMenuOpen ? '17' : '8.5'}`" />
                    <path class="transition-all duration-500 ease-in-out" :d="`m3.5 12h${isMenuOpen ? '17' : '8.5'}`" />
                    <path d="m3.5 17h17" />
                </svg>
                <span
                    class="flex-1 pl-2 ml-2 overflow-hidden leading-normal text-left overflow-ellipsis whitespace-nowrap"
                    >{{ $t('chapters.title') }}</span
                >
            </button>
        </div>

        <ul class="nav-content menu">
            <li>
                <button
                    class="flex items-center px-2 py-1 mx-1"
                    @click="scrollToChapter('intro')"
                    v-tippy="{
                        delay: '200',
                        placement: 'right',
                        content: $t('chapters.return'),
                        animateFill: true,
                        animation: 'chapter-menu',
                        offset: isMenuOpen ? [0, -280] : [0, -40]
                    }"
                    v-if="editor"
                >
                    <svg
                        class="flex-shrink-0"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="#fff"
                        stroke="#878787"
                    >
                        <path
                            d="m19.325 16.229c-2.4415 1.4096-4.8829 2.8191-7.3244 4.2286-2.4415-1.4096-4.883-2.8192-7.3245-4.2288-3.55e-5 -2.8191-7.1e-5 -5.6383-1.066e-4 -8.4574 2.4415-1.4096 4.8829-2.8191 7.3244-4.2286 2.4415 1.4096 4.883 2.8192 7.3245 4.2288 3.7e-5 2.8191 7.4e-5 5.6383 1.1e-4 8.4574z"
                            stroke-width=".93974"
                        />
                    </svg>
                    <span class="flex-1 ml-4 overflow-hidden leading-normal overflow-ellipsis whitespace-nowrap">{{
                        $t('chapters.return')
                    }}</span>
                </button>

                <router-link
                    :to="{ hash: '#intro' }"
                    class="flex items-center px-2 py-1 mx-1"
                    target
                    v-tippy="{
                        delay: '200',
                        placement: 'right',
                        content: $t('chapters.return'),
                        animateFill: true,
                        animation: 'chapter-menu',
                        offset: isMenuOpen ? [0, -280] : [0, -40]
                    }"
                    v-else
                >
                    <svg
                        class="flex-shrink-0"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="#fff"
                        stroke="#878787"
                    >
                        <path
                            d="m19.325 16.229c-2.4415 1.4096-4.8829 2.8191-7.3244 4.2286-2.4415-1.4096-4.883-2.8192-7.3245-4.2288-3.55e-5 -2.8191-7.1e-5 -5.6383-1.066e-4 -8.4574 2.4415-1.4096 4.8829-2.8191 7.3244-4.2286 2.4415 1.4096 4.883 2.8192 7.3245 4.2288 3.7e-5 2.8191 7.4e-5 5.6383 1.1e-4 8.4574z"
                            stroke-width=".93974"
                        />
                    </svg>
                    <span class="flex-1 ml-4 overflow-hidden leading-normal overflow-ellipsis whitespace-nowrap">{{
                        $t('chapters.return')
                    }}</span>
                </router-link>
            </li>
            <li v-for="(slide, idx) in slides" :key="idx" :class="{ 'is-active': activeChapterIndex === idx }">
                <!-- using router-link causes a page refresh which breaks editor preview mode -->
                <button
                    class="flex items-center px-2 py-1 mx-1"
                    @click="scrollToChapter(`${idx}-${slide.title.toLowerCase().replaceAll(' ', '-')}`)"
                    v-tippy="{
                        delay: '200',
                        placement: 'right',
                        content: slide.title,
                        animateFill: true,
                        animation: 'chapter-menu',
                        offset: isMenuOpen ? [0, -280] : [0, -40]
                    }"
                    v-if="editor"
                >
                    <svg
                        class="flex-shrink-0"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="#fff"
                        stroke="#878787"
                    >
                        <path
                            d="m19.325 16.229c-2.4415 1.4096-4.8829 2.8191-7.3244 4.2286-2.4415-1.4096-4.883-2.8192-7.3245-4.2288-3.55e-5 -2.8191-7.1e-5 -5.6383-1.066e-4 -8.4574 2.4415-1.4096 4.8829-2.8191 7.3244-4.2286 2.4415 1.4096 4.883 2.8192 7.3245 4.2288 3.7e-5 2.8191 7.4e-5 5.6383 1.1e-4 8.4574z"
                            stroke-width=".93974"
                        />
                    </svg>
                    <span class="flex-1 ml-4 overflow-hidden leading-normal overflow-ellipsis whitespace-nowrap">{{
                        slide.title
                    }}</span>
                </button>

                <router-link
                    :to="{ hash: `#${idx}-${slide.title.toLowerCase().replaceAll(' ', '-')}` }"
                    class="flex items-center px-2 py-1 mx-1"
                    target
                    v-tippy="{
                        delay: '200',
                        placement: 'right',
                        content: slide.title,
                        animateFill: true,
                        animation: 'chapter-menu',
                        offset: isMenuOpen ? [0, -280] : [0, -40]
                    }"
                    v-else
                >
                    <svg
                        class="flex-shrink-0"
                        width="24"
                        height="24"
                        viewBox="0 0 24 24"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="#fff"
                        stroke="#878787"
                    >
                        <path
                            d="m19.325 16.229c-2.4415 1.4096-4.8829 2.8191-7.3244 4.2286-2.4415-1.4096-4.883-2.8192-7.3245-4.2288-3.55e-5 -2.8191-7.1e-5 -5.6383-1.066e-4 -8.4574 2.4415-1.4096 4.8829-2.8191 7.3244-4.2286 2.4415 1.4096 4.883 2.8192 7.3245 4.2288 3.7e-5 2.8191 7.4e-5 5.6383 1.1e-4 8.4574z"
                            stroke-width=".93974"
                        />
                    </svg>
                    <span class="flex-1 ml-4 overflow-hidden leading-normal overflow-ellipsis whitespace-nowrap">{{
                        slide.title
                    }}</span>
                </router-link>
            </li>
            <div class="h-10 flex-shrink-0"></div>
        </ul>
    </div>
</template>

<script setup lang="ts">
import type { PropType } from 'vue';
import { ref } from 'vue';
import { Slide } from '@storylines/definitions';

defineProps({
    slides: {
        type: Array as PropType<Array<Slide>>,
        required: true
    },
    activeChapterIndex: {
        type: Number,
        required: true
    },
    lang: {
        type: String,
        required: true
    },
    editor: {
        type: Boolean,
        required: true
    }
});

const isMenuOpen = ref(false);

const scrollToChapter = (id: string): void => {
    const el = document.getElementById(id);
    if (el) {
        el.scrollIntoView({ behavior: 'smooth' });
    }
};
</script>

<style lang="scss" scoped>
.nav-bar {
    max-height: calc(100vh - 4rem);
    display: flex;
    flex-direction: column;
}

.nav-content {
    overflow-y: auto;
    -ms-overflow-style: none; /* Internet Explorer 10+ */
    scrollbar-width: none; /* Firefox */
}

.nav-content::-webkit-scrollbar {
    display: none; /* Safari and Chrome */
}

.menu li {
    a:hover {
        text-decoration: none;
        color: inherit;
    }

    a:focus {
        text-decoration: none;
        color: inherit;
    }

    a:hover svg {
        stroke: var(--sr-accent-colour);
    }

    a:visited {
        color: inherit;
    }

    &.is-active {
        svg {
            fill: var(--sr-accent-colour);
            stroke: var(--sr-accent-colour);
        }

        span {
            font-weight: bold;
        }
    }
}
</style>
