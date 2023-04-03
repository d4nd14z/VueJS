<script setup>

    defineProps(['pager', 'total']);

    const emit = defineEmits(['prevHandler', 'nextHandler']);

    const prev = () => {                
        emit('prevHandler');
    }

    const next = () => {        
        emit('nextHandler');
    }

    //Deshabilitar el boton "previous" cuando se llega al primer resultado
    const lockPrevious = (pager) => {        
        return (pager.start <= 0) ? true : false;
    }   

    //Deshabilitar el boton "next" cuando se llega al ultimo resultado
    const lockNext = (pager) => {
        return (pager.end >= pager.total) ? true : false;
    }

</script>

<template>
    <div class="inline-flex mb-4 text-center">
        <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l" @click="prev" :disabled="lockPrevious(pager)">
            Prev
        </button>
        <button class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-r" @click="next" :disabled="lockNext(pager)">
            Next
        </button>              
    </div>
</template>

<style scoped></style>