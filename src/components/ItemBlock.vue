<script setup lang="ts">
import type { InventoryItem } from '@/types';

interface IProps {
    isSelected: boolean;
    inventoryItem: InventoryItem;
}

defineProps<IProps>();
</script>

<template>
    <button type="button" class="item-block" :class="{ 'item-block--select': isSelected }">
        <div class="item-block__id">
            {{ inventoryItem.id }}
        </div>
        <div>
            {{ inventoryItem.name }}
        </div>
    </button>
</template>

<style lang="scss" scoped>
.item-block {
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 1em;
    max-width: 150px;
    min-height: 50px;
    width: 100%;
    height: fit-content;
    padding: 10px;
    border: 1px solid var(--color-border);
    border-radius: var(--border-radius);
    color: var(--color-text);
    background-color: var(--color-background-soft);
    overflow: hidden;
    word-break: break-all;
    transition:
        background-color ease-in-out 0.2s,
        background-color ease-in-out 0.2s;
    &:hover {
        background-color: var(--color-background-mute);
    }
    &.item-block--select {
        border-color: var(--color-accent);
    }
    &:active {
        color: var(--color-accent);
    }

    .item-block__id {
        min-width: 21px;
        border-radius: 5px;
        border: 1px solid var(--color-border);
        padding: 2px;
    }
}
</style>
