<template>
    <div class="py-24 mx-auto text-center max-w-9xl" id="intro">
        <img
            v-if="config.logo && config.logo.src"
            class="inline-block"
            :src="config.logo.src"
            :alt="config.logo.altText"
        />

        <h1 class="m-10 text-5xl font-bold text-gray-800">
            {{ config.title }}
        </h1>
        <p class="w-1/2 m-auto text-2xl font-semibold text-gray-500">
            {{ config.subtitle }}
        </p>

        <!-- using router-link causes a page refresh which breaks editor preview mode -->
        <button @click="scrollToStory" v-if="!!configFileStructure">
            <svg
                class="w-auto h-24 m-auto"
                width="90"
                height="104.84"
                viewBox="0 0 90 104.83"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="m89.51 77.659-44.51 25.698-44.51-25.698 3.86e-4 -51.395 44.51-25.698 44.51 25.698z"
                    fill="#fff"
                    stroke="#00d2d3"
                    stroke-dasharray="4.8960465, 4.8960465"
                    stroke-dashoffset="2.7"
                    stroke-width=".5"
                />
                <path
                    d="m45 104.27-44.51-25.697v-10.646l44.51 25.697 44.51-25.697v10.646z"
                    fill="#00d2d3"
                    stroke="#00d2d3"
                    stroke-width=".97921"
                />
            </svg>
        </button>

        <router-link
            :to="{ hash: '#story' }"
            class="inline-block mt-10 scroll-arrow"
            title="scroll to story"
            target
            v-else
        >
            <svg
                class="w-auto h-24 m-auto"
                width="90"
                height="104.84"
                viewBox="0 0 90 104.83"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="m89.51 77.659-44.51 25.698-44.51-25.698 3.86e-4 -51.395 44.51-25.698 44.51 25.698z"
                    fill="#fff"
                    stroke="#00d2d3"
                    stroke-dasharray="4.8960465, 4.8960465"
                    stroke-dashoffset="2.7"
                    stroke-width=".5"
                />
                <path
                    d="m45 104.27-44.51-25.697v-10.646l44.51 25.697 44.51-25.697v10.646z"
                    fill="#00d2d3"
                    stroke="#00d2d3"
                    stroke-width=".97921"
                />
            </svg>
        </router-link>
    </div>
</template>

<script setup lang="ts">
import type { PropType } from 'vue';
import { getCurrentInstance, onMounted } from 'vue';
import { ConfigFileStructure, Intro } from '@storylines/definitions';

const props = defineProps({
    config: {
        type: Object as PropType<Intro>,
        required: true
    },
    configFileStructure: {
        type: Object as PropType<ConfigFileStructure>
    }
});

onMounted(() => {
    // obtain logo from ZIP file if it exists
    if (props.configFileStructure) {
        const logo = props.config.logo?.src;

        if (logo) {
            const logoSrc = `${logo.substring(logo.indexOf('/') + 1)}`;
            const logoFile = props.configFileStructure.zip.file(logoSrc);
            if (logoFile) {
                logoFile.async('blob').then((res: Blob) => {
                    props.config.logo.src = URL.createObjectURL(res);
                    getCurrentInstance()?.proxy?.$forceUpdate();
                });
            }
        }
    }
});

const scrollToStory = (): void => {
    const el = document.getElementById('story');
    if (el) {
        el.scrollIntoView({ behavior: 'smooth' });
    }
};
</script>

<style lang="scss"></style>
