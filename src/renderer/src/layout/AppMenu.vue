<script setup>
import { ref } from 'vue';

import AppMenuItem from './AppMenuItem.vue';

const model = ref([
    {
        label: 'ホーム',
        items: [
            { label: 'PDFリスト', icon: 'pi pi-fw pi-home', to: '/' },
            { label: '出力テンプレート', icon: 'pi pi-fw pi-home', to: '/json_template' },
            { label: '出力先', icon: 'pi pi-fw pi-home', to: '/export_system' },
        ]
    }
]);
</script>

<template>
    <ul class="layout-menu">
        <template v-for="(item, i) in model" :key="item">
            <app-menu-item v-if="!item.separator" :item="item" :index="i"></app-menu-item>
            <li v-if="item.separator" class="menu-separator"></li>
        </template>
    </ul>
</template>

<style lang="scss" scoped></style>
