<template>
    <div>
        <div class="text-white mb-4">
            <template v-if="winner >= 0 && winner < 3"><span class="font-medium">Winner</span>: Player {{keys[winner] }}</template>
            <template v-else-if="winner === 3">Empate</template>
            <button class="px-2 py-0 text-black bg-white ml-2" v-if="winner !== -1" @click="reset" >
                 reset
            </button>
        </div>
       
        <div class="w-[300px] h-[300px] grid grid-cols-3">
            
            <TButton v-for="(item,index) in unifiedLines" @click="play(index)" :color="item === 0? 'green': 'red'">{{ item >= 0 ? keys[item] : '' }}</TButton>
            
        </div>
    </div>
   
</template>
<script setup>
import {ref,computed} from 'vue'
import TButton from './TButton.vue'
const winsPossibilities = [
    [[0,0],[0,1],[0,2]],
    [[0,0],[1,0],[2,0]],
    [[2,0],[2,1],[2,2]],
    [[0,2],[1,2],[2,2]],
    [[0,1],[1,1],[2,1]],
    [[1,0],[1,1],[1,2]],
    [[0,0],[1,1],[2,2]],
    [[0,2],[1,1],[2,0]],
];
const keys = {
    0: 'X',
    1: 'O'
}
const board = ref([[-1,-1,-1],[-1,-1,-1],[-1,-1,-1]]);
const unifiedLines = computed(() => board.value.reduce((acc,columns) => [...acc,...columns],[]))
const turn = ref(0);
const winner = ref(-1);


const getCoordinates = (index) => {
    const x = parseInt(index/3);
    const y = index % 3;
    return { x,y }
}

const play = (index) => {
    const { x,y } = getCoordinates(index);

    board.value[x][y] = turn.value;
    turn.value = Number(!Boolean(turn.value));
    verifyWinner();
}

const reset = () => {
    for (let i = 0; i < board.value.length; i++) {
        for (let j = 0; j < board.value[i].length; j++) {
            board.value[i][j] = -1;
        }
    }
 
    winner.value = -1;
}

const verifyWinner = () => {
    for (const winPossibility of winsPossibilities) {
        let previousElement = -1;
        let isWinner = true;
        for (const [x, y] of winPossibility) {
            const element = board.value[x][y];
            
            if (previousElement !== -1 && previousElement !== element) {
                isWinner = false;
                break;
            }
            
            previousElement = element;
        }
        
        if (isWinner && previousElement !== -1) {
            winner.value = previousElement;
            return;
        }
    }
    
    const isTie = unifiedLines.value.every(item => item !== -1);
    if (isTie) {
        winner.value = 2;
    }
}





</script>