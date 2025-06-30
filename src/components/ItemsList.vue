<script setup lang="ts">
import type { InventoryItem } from '@/types';

import ItemBlock from '@/components/ItemBlock.vue';

interface IProps {
    listTitle: string;
    inventoryList: InventoryItem[];
    selectedIdList?: number[];
}

const props = defineProps<IProps>();

const emit = defineEmits<{ (e: 'clickItem', id: number): void }>();

function checkSelected(id: number) {
    return props.selectedIdList?.includes(id) || false;
}

function handleClick(id: number) {
    emit('clickItem', id);
}
</script>

<template>
    <div class="list-container">
        <h2 class="list-title">{{ listTitle }}</h2>
        <div v-if="inventoryList.length" class="items-list">
            <TransitionGroup name="list">
                <ItemBlock
                    v-for="item in inventoryList"
                    :key="item.id"
                    :is-selected="checkSelected(item.id)"
                    :inventory-item="item"
                    @click="handleClick(item.id)"
                />
            </TransitionGroup>
        </div>
        <div v-else class="items-list items-list--empty">Ничего не выбрано</div>
    </div>
</template>

<style lang="scss" scoped>
.list-container {
    display: grid;
    grid-template-rows: auto 1fr;
    width: 100%;
    height: 100%;
}
.list-title {
    font-size: 1.3rem;
    font-weight: 700;
    color: var(--color-heading);
}
.items-list {
    height: 100%;
    width: 100%;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
    gap: 12px;
    border: 1px solid var(--color-border);
    border-radius: var(--border-radius);
    padding: 16px;
    &.items-list--empty {
        grid-template-columns: 1fr;
    }
}

.list-enter-from {
    opacity: 0;
}
.list-enter-to {
    opacity: 1;
}
.list-enter-active,
.list-leave-active,
.list-move {
    transition: all 0.3s;
}
.list-leave-active {
    position: absolute;
}
.list-leave-from {
    opacity: 1;
}
.list-leave-to {
    opacity: 0;
}

@media (min-width: 768px) {
    .list-title {
        font-size: 1.5rem;
    }
    .items-list {
        gap: 15px;
        padding: 20px;
    }
}
</style>
