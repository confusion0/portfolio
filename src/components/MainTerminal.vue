<script setup>
    import { ref } from 'vue';

    const props = defineProps({
        isOpened: {
            type: Boolean,
            required: true
        },
        zIndex: {
            type: Number,
            required: true
        }
    })

    const terminalLines = ref([]);
    const isTyping = ref(false);
    const isFocused = ref(false);
    const placeholder = ref('Type <span style="color: #A6E22E;">help</span> for commands');

    const commands = {
        'help': `
            <pre>
 ██████╗ ██████╗ ███╗   ███╗███╗   ███╗ █████╗ ███╗   ██╗██████╗ ███████╗
██╔════╝██╔═══██╗████╗ ████║████╗ ████║██╔══██╗████╗  ██║██╔══██╗██╔════╝
██║     ██║   ██║██╔████╔██║██╔████╔██║███████║██╔██╗ ██║██║  ██║███████╗
██║     ██║   ██║██║╚██╔╝██║██║╚██╔╝██║██╔══██║██║╚██╗██║██║  ██║╚════██║
╚██████╗╚██████╔╝██║ ╚═╝ ██║██║ ╚═╝ ██║██║  ██║██║ ╚████║██████╔╝███████║
 ╚═════╝ ╚═════╝ ╚═╝     ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝╚═════╝ ╚══════╝                                                    
            </pre>                                                                        
            <span style="color: #A6E22E;">resume</span>: display my resume!<br>
            <span style="color: #A6E22E;">about me</span>: display some interesting information about me!<br>
            <span style="color: #A6E22E;">projects</span>: tells you about projects i've worked on!<br>
        `,
        'about me': `
        `,
        'projects': `
        <pre>
██████╗ ██████╗  ██████╗      ██╗███████╗ ██████╗████████╗███████╗
██╔══██╗██╔══██╗██╔═══██╗     ██║██╔════╝██╔════╝╚══██╔══╝██╔════╝
██████╔╝██████╔╝██║   ██║     ██║█████╗  ██║        ██║   ███████╗
██╔═══╝ ██╔══██╗██║   ██║██   ██║██╔══╝  ██║        ██║   ╚════██║
██║     ██║  ██║╚██████╔╝╚█████╔╝███████╗╚██████╗   ██║   ███████║
╚═╝     ╚═╝  ╚═╝ ╚═════╝  ╚════╝ ╚══════╝ ╚═════╝   ╚═╝   ╚══════╝                                              
        </pre>
        <ul>
            <li><span style="color: #F92672;">factful.io</span> - A web application capable of fact and spell checking for writing</li>
            <li><span style="color: #F92672;">stop sign</span> - A discord bot capable of integrating various moderation features into a discord server</li>
        </ul>
        `
    };

    function handleEnter(event) {
        const inputValue = event.target.textContent.trim();

        if (inputValue && inputValue !== '') {
            terminalLines.value.push('C:\\Developers\\confusion0>  ' + inputValue);

            if (commands[inputValue] !== undefined) {
                terminalLines.value.push(commands[inputValue])
            } else {
                terminalLines.value.push(`<span style="color: #F92672;">Command not found.</span> Type <span style="color: #A6E22E;">help</span> for the list of commands`)
            }

            event.target.innerHTML = '';
            isTyping.value = false;

            setTimeout(() => {
                terminalLines.value.push();
            }, 500);
        }
    };

    function handleInput(event) {
        const content = event.target?.textContent.trim();

        isTyping.value = content.length > 0;
    };

    function handleFocus() {
        isFocused.value = true;
    };

    function handleBlur() {
        isFocused.value = false;
    };
</script>

<template>
    <div v-if="isOpened" class="window resize-drag" :style="{ zIndex: zIndex }">
        <div class="window-top-bar">
            <div class="window-controls">
                <div class="button control close"></div>
                <div class="button control minimize"></div>
                <div class="button control maximize"></div>
            </div>

            <div class="window-header">Terminal</div>
        </div>
    
        <div class="terminal-container">
            <div class="terminal-content-container">
                <div v-for="(line, index) in terminalLines" :key="index" class="terminal-line" v-html="line" />
            </div>
            
            <div class="terminal-editable-block">
                <div class="terminal-placeholder">C:\Developers\confusion0></div>
                
                <div class="terminal-editable-container">
                    <div 
                        class="terminal" 
                        contenteditable="true" 
                        @keydown.enter.prevent="handleEnter"
                        @input="handleInput"
                        @focus="handleFocus"
                        @blur="handleBlur"
                    />

                    <div v-if="!isTyping && !isFocused" class="placeholder" v-html="placeholder" />
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    @keyframes blinkCursor {
        50% {
            border-right-color: transparent;
        }
    }

    @keyframes typeAndDelete {
        0%,
        10% {
            width: 0;
        }
        45%,
        55% {
            width: 157.25px;
        }
        90%,
        100% {
            width: 0;
        }
    }

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

    .terminal {
        width: 100%;
        height: 100%;
        padding-right: 0;
        outline: none;
        word-wrap: break-word;
        position: absolute;
        z-index: 2;
    }

    .terminal-editable-block {
        padding: 0 10px;
        display: flex;
        align-items: start;
        flex-direction: row;
    }

    .terminal-placeholder {
        padding: 10px 0;
    }

    .terminal-container {
        height: 100%;
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: start;
        font-family: monospace;
        overflow-y: scroll;
        cursor: text;
    }

    .terminal-content-container {
        padding: 10px 10px 0 10px;
        display: flex;
        flex-direction: column;
    }

    .terminal-line {
        color: white;
        padding: 2px 0;
    }

    .terminal-editable-container {
        height: 100%;
        width: 100%;
        padding: 10px;
        position: relative;
    }

    .placeholder {
        filter: brightness(0.7);
        position: absolute;
        overflow: hidden;
        border-right: 0.2em solid;
        animation: typeAndDelete 4s steps(11) infinite, blinkCursor 0.5s step-end infinite alternate;
        white-space: nowrap;
    }
</style>