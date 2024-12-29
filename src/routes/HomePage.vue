<script setup>
  import { reactive, ref } from 'vue';

  import MainTerminal from '../components/MainTerminal.vue'
  import iFrameWindow from '../components/iFrameWindow.vue';

  import interact from '@interactjs/interact';
  import '@interactjs/modifiers'
  import '@interactjs/actions/drag'
  import '@interactjs/actions/resize'
  import '@interactjs/auto-start'

  const maxZIndex = ref(3);
  const windows = reactive([
    { url: 'https://factful.io', zIndex: 1, isOpened: false },
    { url: 'https://stopsign.glitch.me', zIndex: 2, isOpened: false },
  ])

  const bringWindowToFront = (index) => {
    windows[index].zIndex = maxZIndex.value++;
  };

  const mainTerminalIsOpened = ref(true)
  const mainTerminalZIndex = ref(0)

  const bringMainTerminalToFront = () => {
    mainTerminalZIndex.value = maxZIndex.value++;
  }

  function dragMoveListener(event) {
    const target = event.target;
    let x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx;
    let y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy;

    target.style.transform = `translate(${x}px, ${y}px)`;
    target.setAttribute('data-x', x);
    target.setAttribute('data-y', y);
  }

  interact('.resize-drag')
  .resizable({
    edges: { left: true, right: true, bottom: true, top: true },
    listeners: {
      move(event) {
        const target = event.target;
        let x = (parseFloat(target.getAttribute('data-x')) || 0);
        let y = (parseFloat(target.getAttribute('data-y')) || 0);

        target.style.width = `${event.rect.width}px`;
        target.style.height = `${event.rect.height}px`;

        x += event.deltaRect.left;
        y += event.deltaRect.top;

        target.style.transform = `translate(${x}px, ${y}px)`;

        target.setAttribute('data-x', x);
        target.setAttribute('data-y', y);
      }
    },
    modifiers: [
      interact.modifiers.restrictEdges({
        outer: 'parent'
      }),
      interact.modifiers.restrictSize({
        min: { width: 100, height: 50 }
      })
    ],
    inertia: true
  })
  .draggable({
    listeners: { move: dragMoveListener },
    inertia: true,
    modifiers: [
      interact.modifiers.restrictRect({
        restriction: 'parent',
        endOnly: true
      })
    ]
  });
</script>

<template>
    <MainTerminal @click="bringMainTerminalToFront()" :isOpened=mainTerminalIsOpened :zIndex="mainTerminalZIndex" />

    <iFrameWindow v-for="(window, index) in windows" @click="bringWindowToFront(index)" :isOpened="window.isOpened" :zIndex=window.zIndex :url="window.url" :key="index" />
</template>