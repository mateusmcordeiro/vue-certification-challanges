<template>
    <div class="bg-[#374151] w-[262px] h-[413px] rounded-md p-[22px]">
        <div class="w-[217px] h-[54px] bg-[#14532D] mb-[32px] py-3 px-6 text-white text-xl">
            {{ displayOperation }} <template v-if="result"> = {{ result }}</template>
        </div>
        <div class="grid grid-cols-4 gap-2">
            <CButton @click="clear">C</CButton>
            <div></div>
            <div></div>
            <CButton @click="callOperation('/')">/</CButton>
            <CButton @click="fillNumber('7')">7</CButton>
            <CButton @click="fillNumber('8')">8</CButton>
            <CButton @click="fillNumber('9')">9</CButton>
            <CButton @click="callOperation('*')">x</CButton>
            <CButton @click="fillNumber('4')">4</CButton>
            <CButton @click="fillNumber('5')">5</CButton>
            <CButton @click="fillNumber('6')">6</CButton>
            <CButton @click="callOperation('-')">-</CButton>
            <CButton @click="fillNumber('1')">1</CButton>
            <CButton @click="fillNumber('2')">2</CButton>
            <CButton @click="fillNumber('3')">3</CButton>
            <CButton @click="callOperation('+')">+</CButton>
            <CButton @click="fillNumber('0')">0</CButton>
            <div></div>
            <CButton @click="callOperation('.')">.</CButton>
            <CButton @click="getResults">=</CButton>
        </div>
    </div>
</template>
<script setup>
import {ref,computed} from 'vue'
import CButton from './CButton.vue'

const keyDictionary = {
    '*': 'x',
    'x': '*',
    '/':'÷',
    '÷': '/'
}

const forbiddenTogether = ['/','*']
const operations = [...forbiddenTogether,'-','+']

const stack = ref([]);
const result = ref('');


const displayOperation = computed(() => {
    const items = stack.value.map(char => keyDictionary[char] || char)
    return items.join('')
})


const clear = () => {
    stack.value = []
    result.value = ''
}


const callOperation = (operation) => {
 
    if (!stack.value.length && forbiddenTogether.includes(operation)) {
        return
    }
    const lastItem = stack.value[stack.value.length - 1] ;
    if (forbiddenTogether.includes(operation) && forbiddenTogether.includes(lastItem)) {
        stack.value[stack.value.length - 1] = operation;
        return
    }
    stack.value.push(operation);
    if (result.value.length) {
        result.value = ''
    };
}

const fillNumber = (number) => {
    const lastNumber = stack.value.at(-1)
    if((!stack.value.length  || (!isNaN(lastNumber) && lastNumber == '0'  )|| operations.includes(lastNumber)) && number == '0'){
        return;
    }
    stack.value.push(number);
}

const getResults = () => {
    result.value = eval(stack.value.join(''))
}
</script>