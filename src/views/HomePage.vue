<script setup lang="ts">
import { computed, onBeforeMount, ref } from 'vue';

import type { InventoryItem } from '@/types';

import ItemsList from '@/components/ItemsList.vue';

const userInventory = ref<InventoryItem[]>([]);
const otherInventory = ref<InventoryItem[]>([]);

const selectedUserInventory = ref<InventoryItem[]>([]);
const selectedUserInventoryIds = computed(() => {
    return selectedUserInventory.value.map((el) => el.id);
});

const selectedOtherInventory = ref<InventoryItem[]>([]);
const selectedOtherInventoryIds = computed(() => {
    return selectedOtherInventory.value.map((el) => el.id);
});

const userInventoryErrorMessage = ref('');

onBeforeMount(async () => {
    try {
        const [responseUserInventory, responseOtherInventory] = await Promise.all([
            fetch('/userInventory.json', { method: 'get' }),
            fetch('/otherInventory.json', { method: 'get' }),
        ]);
        userInventory.value = await responseUserInventory.json();
        otherInventory.value = await responseOtherInventory.json();
    } catch (error) {
        console.log('Нет json файлов на месте. ', error);
    }
});

function handleClickUserInventoryItem(id: number) {
    const currentInventoryItem = userInventory.value.find((el) => el.id === id);
    if (!currentInventoryItem) {
        return;
    }
    if (selectedUserInventoryIds.value.includes(id)) {
        selectedUserInventory.value = selectedUserInventory.value.filter((el) => el.id !== id);
    } else {
        if (selectedUserInventory.value.length >= 6) {
            userInventoryErrorMessage.value = 'Не более 6 предметов';
            setTimeout(() => {
                userInventoryErrorMessage.value = '';
            }, 2000);
            return;
        }
        selectedUserInventory.value.push(currentInventoryItem);
    }
}

function handleClickOtherInventoryItem(id: number) {
    const currentInventoryItem = otherInventory.value.find((el) => el.id === id);
    if (!currentInventoryItem) {
        return;
    }
    if (selectedOtherInventoryIds.value.includes(id)) {
        selectedOtherInventory.value = selectedOtherInventory.value.filter((el) => el.id !== id);
    } else {
        selectedOtherInventory.value = [currentInventoryItem];
    }
}
</script>

<template>
    <main class="main">
        <div class="users-items-container">
            <div>
                <ItemsList
                    list-title="Мой выбор (набор вещей)"
                    :inventory-list="selectedUserInventory"
                    class="selected-area"
                    @click-item="handleClickUserInventoryItem"
                />
                <div v-if="userInventoryErrorMessage" class="error-message">
                    {{ userInventoryErrorMessage }}
                </div>
            </div>
            <ItemsList
                list-title="Все мои вещи"
                :inventory-list="userInventory"
                :selected-id-list="selectedUserInventoryIds"
                @click-item="handleClickUserInventoryItem"
            />
        </div>
        <div class="other-items-container">
            <ItemsList
                list-title="Выбранная вещь"
                :inventory-list="selectedOtherInventory"
                class="selected-area"
                @click-item="handleClickOtherInventoryItem"
            />
            <ItemsList
                list-title="Доступные вещи на выбор"
                :inventory-list="otherInventory"
                :selected-id-list="selectedOtherInventoryIds"
                @click-item="handleClickOtherInventoryItem"
            />
        </div>
    </main>
</template>

<style lang="scss" scoped>
.main {
    display: grid;
    grid-template-columns: 1fr;
    gap: 60px;
}
.users-items-container,
.other-items-container {
    position: relative;
    width: 100%;
    display: grid;
    grid-template-columns: 1fr;
    gap: 10px;
}
.error-message {
    color: red;
    position: absolute;
}

@media (min-width: 768px) {
    .main {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 50px;
    }

    .selected-area {
        min-height: 200px;
    }
    .users-items-container,
    .other-items-container {
        gap: 30px;
    }
}
</style>
