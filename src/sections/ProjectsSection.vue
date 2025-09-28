<template>
    <section :id="id" class="section projects">
        <h2 class="title">projects</h2>
        <p>---施工中---</p>
    </section>
</template>

<script setup lang="ts">
defineProps<{ id: string }>()
</script>

<style scoped>
.projects p {
    margin: auto;
}
</style>
