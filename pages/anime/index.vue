<template>
    <div class="large centered row">
        <div class="half col">
            <pre class="large log row">
      <span class="label">current time</span>
      <span class="value lcd">{{ timer }}</span>
    </pre>
        </div>
        <div class="half col">
            <pre class="large log row">
      <span class="label">callback fired</span>
      <span class="value lcd">{{ count }}</span>
    </pre>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { animate } from 'animejs';
import { createTimer } from 'animejs';

const timer = ref(0);
const count = ref(0);

createTimer({
    duration: 1000,
    loop: true,
    frameRate: 30,
    onUpdate: self => timer.value = self.currentTime,
    onLoop: self => count.value = self._currentIteration
});


</script>