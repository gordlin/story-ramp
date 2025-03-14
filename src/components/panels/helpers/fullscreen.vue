<template>
    <div ref="el">
        <div class="relative bg-white fullscreen-wrapper rounded-lg">
            <button
                v-if="expandable !== undefined ? expandable : true"
                class="fullscreenButton expand-button absolute items-center justify-center p-3 z-10"
                :class="[fullscreen ? `top-0` : `bottom-0`, type === 'image' ? `right-10` : `right-2`]"
                :aria-label="$t('image.fullscreen')"
                :content="$t(fullscreen ? 'fullscreen.deactivate' : 'fullscreen.activate')"
                v-tippy="{ placement: 'top', hideOnClick: false, animateFill: true }"
                @click="toggleFullscreen"
            >
                <svg
                    v-show="fullscreen"
                    xmlns="http://www.w3.org/2000/svg"
                    height="24px"
                    viewBox="0 0 24 24"
                    width="24px"
                    fill="#595959"
                >
                    <path
                        d="M19,6.41L17.59,5L12,10.59L6.41,5L5,6.41L10.59,12L5,17.59L6.41,19L12,13.41L17.59,19L19,17.59L13.41,12L19,6.41Z"
                        fill="#666666"
                    />
                </svg>
                <svg
                    v-show="!fullscreen"
                    xmlns="http://www.w3.org/2000/svg"
                    height="24px"
                    viewBox="0 0 24 24"
                    width="24px"
                    fill="#595959"
                >
                    <path
                        d="M10,21V19H6.41L10.91,14.5L9.5,13.09L5,17.59V14H3V21H10M14.5,10.91L19,6.41V10H21V3H14V5H17.59L13.09,9.5L14.5,10.91Z"
                    />
                </svg>
            </button>
            <slot></slot>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { api as $fullscreen } from 'vue-fullscreen';

defineProps({
    expandable: {
        type: Boolean
    },
    type: {
        type: String
    }
});

const fullscreen = ref<boolean | undefined>(false);
const el = ref();

const toggleFullscreen = async (): Promise<void> => {
    await $fullscreen.toggle((el.value as Element).querySelector('.fullscreen-wrapper'), {
        teleport: true,
        pageOnly: true,
        fullscreenClass: 'fullscreenElement'
    });
    fullscreen.value = $fullscreen.isFullscreen;
};
</script>

<style>
.fullscreenElement {
    z-index: 100;
    background: #000;
}
.fullscreenButton {
    filter: invert(1);
    mix-blend-mode: difference;
}
</style>
