<script setup>
// import HelloWorld from './components/HelloWorld.vue'
import findVolume from './components/findVolume.vue'
</script>

<template>
  <header>
    <p>VSUCA</p>
  </header>

  <main>
    <find-volume />
  </main>
</template>

<style scoped>
header {
  background-color: var(--background-color-inverted);
  color: var(--color-inverted-fixed);
  margin: 0;
  font-size: 2em;
  padding: 0 20%;
}

header p {
  margin: 0;
}

main {
  padding: 1em 20%;
}

</style>
