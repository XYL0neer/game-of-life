<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">
          <h1>Conway's Game of Life</h1>
        </RouterLink>
      </nav>
    </div>
  </header>
  <RouterView />
</template>
