<script setup lang="ts">
import { useUrlSearchParams } from '@vueuse/core';
import GameOfLife from '@/components/GameOfLife.vue';

const params = useUrlSearchParams("history", {
  write: false,
})
const height = parseInt(params.height?.toString() ?? "10", 10)
const width = parseInt(params.width?.toString() ?? "10", 10)
const initialLifePool = parseInt(params.initialLifePool?.toString() ?? "30", 10)
</script>


<template>
  <GameOfLife :height="height" :width="width" :initialLifePool="initialLifePool" />
</template>
