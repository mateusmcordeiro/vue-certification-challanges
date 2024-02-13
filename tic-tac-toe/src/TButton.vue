<template>
    <button v-bind="$attrs" class="bg-[#1D2530] w-[97px] h-[97px] flex items-center justify-center text-white text-3xl" :class="[
        color === 'red'?'text-[red]': 'text-[green]'
    ]">
        <slot/>
    </button>
</template>
<script setup>
import {defineProps} from 'vue'
    defineProps({
        color: String
    })
</script>
