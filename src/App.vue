<script setup lang="ts">
import HomePage from './views/HomePage.vue';
</script>

<template>
    <header>
        <div class="wrapper">
            <h1>Тестовое задание</h1>
            <div>
                <a href="https://zamdikhanov.ru" target="_blank" class="app-link">Обо мне</a>
            </div>
        </div>
    </header>

    <HomePage />
</template>

<style scoped lang="scss">
header {
    margin-bottom: 4rem;
}
.app-link {
    text-decoration: none;
    color: var(--color-accent);
    background-color: transparent;
    &:hover {
        text-decoration: underline;
    }
}
</style>
