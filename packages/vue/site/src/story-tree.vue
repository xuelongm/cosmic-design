<template>
    <div class="my-10">Default - size=md</div>
    <tree
        :data="treedata"
        size="md"
        class="m-10"
        @click-node="select"
    />

    <div class="my-10">Simple Customlized  - size=sm -light</div>
    <tree
        :data="treedata"
        size="sm"
        class="m-10"
        :styles="treeLight"
        @click-node="select"
    >
        <template #arrow="slotProps">
            <span v-if="slotProps.state == TreeNodeState.open" class="inline-block w-10">▾</span>
            <span v-if="slotProps.state == TreeNodeState.close" class="inline-block w-10">▸</span>
        </template>
        <template #prefix="slotProps">
            <span v-if="slotProps.nodeData.children" class="inline-block w-10">✿</span>
            <span v-if="!slotProps.nodeData.children" class="inline-block w-10">❀</span>
        </template>
        <template #label="slotProps">
            {{ slotProps.nodeData.label }}
        </template>
        <template #subfix>
            <i-cosmic-eye-open />
        </template>
    </tree>

    <div class="my-10">Editor Customlized - Editable - Secondary</div>
    <tree
        editable
        class="m-10 customlized"
        :data="treedata"
        :styles="treeSecondary"
        @click-node="select"
        @change-label="change"
    >
        <template #prefix="slotProps">
            <i-cosmic-board v-if="slotProps.nodeData.children" />
            <i-cosmic-rounded-square v-if="!slotProps.nodeData.children" />
        </template>
        <template #label="slotProps">
            {{ slotProps.nodeData.label }}
        </template>
        <template #subfix>
            <i-cosmic-eye-open />
        </template>
    </tree>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import {
    Tree,
    TreeNodeState,
    type TreeNodeEvent,
    type TreeChangeEvent,
} from 'cosmic-vue';

import { treeLight, treeSecondary } from 'cosmic-ui';

const treedata = ref([
    {
        label: '0-0',
        id: '0-0',
        readonly: '1',
        children: [
            {
                label: '0-0-0',
                id: '0-0-0',
                open: '0',
                children: [
                    { label: '0-0-0-0', id: '0-0-0-0' },
                    { label: '0-0-0-1', id: '0-0-0-1' },
                    { label: '0-0-0-2', id: '0-0-0-2' },
                ],
            },
            {
                label: '0-0-2',
                id: '0-0-2',
            },
        ],
    },
    {
        label: '0-1',
        id: '0-1',
        open: '0',
        children: [
            { label: '0-1-0-0', id: '0-1-0-0' },
            { label: '0-1-0-1', id: '0-1-0-1' },
            { label: '0-1-0-2', id: '0-1-0-2' },
        ],
    },
    {
        label: '0-2',
        id: '0-2',
    },
]);

function select(event: TreeNodeEvent) {
    treedata.value = changeData(treedata.value, '', 'selected', '');
    treedata.value = changeData(treedata.value, event.id, 'selected', '1');
}

function change(event: TreeChangeEvent) {
    const key = event.id;
    const label = event.label;
    treedata.value = changeData(treedata.value, key, 'label', label);
}

function changeData(arr: any, id: string, key: string, value: string) {
    arr.forEach((obj: any) => {
        if(obj.id === id || id == '') {
            obj[key] = value;
        }
        if(obj.children && obj.children.length) {
            obj.children = changeData(obj.children, id, key, value);
        }
    });
    return arr;
}
</script>
<style>
    .customlized {
        --font-md : 1.2rem;
        --icon-md : 1.4rem;
    }
</style>