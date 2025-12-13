<script setup>
    defineProps({
        restartGame: {
            type: Function,
            required: true
        }
    })
</script>

<template>
    <div class="rounded-2xl bg-white shadow-2xl border border-neutral-200">
        <div class="p-6 space-y-8 text-center">
        <h2 class="text-2xl font-bold text-neutral-800">No More Moves Available</h2>
        <p class="text-sm text-neutral-600">
            The knight has no more possible moves left. Please restart the game to try again.
        </p>
        <button 
            @click="restartGame" 
            class="px-6 py-3 bg-sky-500 text-white font-semibold rounded-lg hover:bg-sky-600 transition-colors">
            Restart Game
        </button>
        </div>
    </div>
</template>