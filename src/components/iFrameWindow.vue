<script setup>
    const props = defineProps({
        isOpened: {
            type: Boolean,
            required: true
        },
        zIndex: {
            type: Number,
            required: true
        },
        url: {
            type: String,
            required: true
        }
    })
</script>

<template>
    <div v-if="isOpened" class="window resize-drag" :style="{ zIndex: zIndex }">
        <div class="window-top-bar">
            <div class="window-controls">
                <div class="button control close"></div>
                <div class="button control minimize"></div>
                <div class="button control maximize"></div>
            </div>

            <div class="window-header">{{ url }}</div>
        </div>
        
        <iframe :src="url"></iframe>
    </div>
</template>

<style scoped>
    .window {
        position: absolute;
        height: 50%;
        width: 50%;
        background: black;
        z-index: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        border-radius: 10px;
        overflow: hidden;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .window-top-bar {
        width: 100%;
        height: 30px;
        background: #333;
        display: grid;
        grid-template-columns: 33% 33% 33%;
        align-items: center;
        padding-left: 10px;
    }

    .window-header {
        justify-self: center;
        font-family: monospace;
    }

    .window-controls {
        display: flex;
        align-items: center;
        flex-direction: row;
        gap: 0.1rem;
    }

    .button {
        cursor: pointer;
    }

    .button:hover {
        filter: brightness(0.7);
    }

    .control {
        width: 0.6em;
        height: 0.6em;
        margin-left: 0.4em;
        border-radius: 50%;
        background-color: #777;
    }

    .close {
        background-color: #e33;
    }

    .minimize {
        background-color: #ee0;
    }

    .maximize {
        background-color: #0b0;
    }

    iframe {
        height: 100%;
        width: 100%;
    }
</style>