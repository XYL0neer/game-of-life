<script setup lang="ts">
import { useUrlSearchParams } from '@vueuse/core';
import GameOfLife from '@/components/GameOfLife.vue';

const params = useUrlSearchParams()
const height = parseInt(params.height, 10)
const width = parseInt(params.width, 10)
const initialLifePool = parseInt(params.initialLifePool, 10)
</script>


<template>
  <GameOfLife :height="height" :width="width" :initialLifePool="initialLifePool" />
</template>
