<script setup>
    defineProps({
        isOpened: {
            type: Boolean,
            required: true
        }
    })
</script>

<template>
    <div v-if="isOpened" class="window resize-drag">
        <div class="window-top-bar">
    
        </div>
    
        <div class="terminal-container">
            <div class="terminal-editable-block">
                <div class="terminal-placeholder">C:\Developers\confusion0></div>
        
                <div class="terminal" contenteditable="true">
                </div>
            </div>
        </div>
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
  }

  .window-top-bar {
    width: 100%;
    height: 20px;
    background: white;
  }

  .terminal {
    height: auto;
    width: 100%;
    padding: 10px;
    padding-right: 0;
    background-color: black;
    outline: none;
    overflow-wrap: anywhere;

    word-wrap: break-word;
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
    flex-direction: column-reverse;
    justify-content: start;
    font-family: monospace;
    overflow-y: scroll;
    cursor: text;
  }

  textarea {
    width: 100%;
    height: 100%;
    resize: none;
    border: none;
    outline: none;
    padding: 10px;
  }
</style>