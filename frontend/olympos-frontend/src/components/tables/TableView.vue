<template>
    <div>
        <table>
            <thead>
                <tr id="theadRow">
                    <th v-for="(item, index) in colDef" :key="index" :style="{width: item.ratio*10+'px'}">{{ item.header }}</th>
                </tr>
            </thead>
            <tbody id="tbody">
                <tr v-for="(item, index) in rowData" :key="index">
                    <td v-for="header in colDef" :key="header.value">
                        <template v-if="header.value === 'number'">
                            {{ index + 1 }}
                        </template>

                        <component v-else-if="header.extend === true" :is="item[header.value].component">
                            <slot>{{item[header.value].slot}}</slot>
                        </component>
                        
                        <template v-else>
                            {{ item[header.value] }}
                        </template>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup lang="ts">
const props = defineProps({
    colDef: Object,
    rowData: Object
})
</script>

<style scoped>
table {
    width: 100%;
    text-align: left;
    font-size: 12px;
    border-collapse: collapse;
}
tr {
    height: 47px;
}
tbody tr {
    height: 45px;
    border-bottom: 1px solid var(--main-gray-3);
}
tbody tr:hover {
    background-color: var(--main-mute-1);
}
</style>