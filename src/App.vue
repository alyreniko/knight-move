<script setup>
  import { computed, reactive } from 'vue';
  import NoMoves from './components/NoMoves.vue';
  import knightImg from './assets/knight.png';

  let board = Array.from({ length: 8 }, () => Array(8).fill(false));

  const knightPos = reactive({ x: 0, y: 0 });
  const gameState = reactive({ isGameOver: false });

  const possibleMoves = [
    { x: 2, y: 1 }, { x: 1, y: 2 }, { x: -1, y: 2 }, { x: -2, y: 1 },
    { x: -2, y: -1 }, { x: -1, y: -2 }, { x: 1, y: -2 }, { x: 2, y: -1 }
  ];

  const isShowPossibleMove = true;

  const possibleTargets = computed(() => {
    const set = new Set();
    for (const m of possibleMoves) {
      const tx = knightPos.x + m.x;
      const ty = knightPos.y + m.y;
      if (tx >= 0 && tx < 8 && ty >= 0 && ty < 8 && !board[tx][ty]) {
        set.add(`${tx},${ty}`);
      }
    }
    if (set.size === 0) gameState.isGameOver = true;
    return set;
  });

  const isPossibleCell = (x, y) => possibleTargets.value.has(`${x},${y}`);

  const moveKnightTo = (x, y) => {
    if (!isPossibleCell(x, y)) return;
    board[knightPos.x][knightPos.y] = true;
    knightPos.x = x;
    knightPos.y = y;
  };

  const restartGame = () => {
    board = Array.from({ length: 8 }, () => Array(8).fill(false));
    knightPos.x = 0;
    knightPos.y = 0;
    gameState.isGameOver = false;
  };
</script>

<template class=" ">
  <div class="w-screen h-screen flex items-center justify-center">
    <div class="p-4 height-[96vmin] w-[96vmin] max-w-full max-h-full rounded-2xl bg-white shadow-xl border border-neutral-200" >
      <div
        class="grid grid-cols-8 gap-1"
      >
        <template v-for="y in 8" :key="y">
          <template v-for="x in 8" :key="x">
            <div
              :class="[
                (x + y) % 2 === 0 ? 'bg-green-100' : 'bg-sky-100',
                isShowPossibleMove && isPossibleCell(x - 1, y - 1)
                  ? 'border-2 border-neutral-400 cursor-pointer'
                  : 'border border-sky-200',
                'cell rounded-sm text-neutral-700 flex items-center justify-center shadow-sm aspect-square',
              ]"
              :style="{ visibility: board[x - 1][y - 1] ? 'hidden' : 'visible' }"
              @click="moveKnightTo(x - 1, y - 1)"
            >
              <img
                v-if="knightPos.x === x - 1 && knightPos.y === y - 1"
                :src="knightImg"
                class="pointer-events-none w-full h-full object-contain p-1"
              />
              <div
                v-else-if="isShowPossibleMove && isPossibleCell(x - 1, y - 1)"
                class="select-none rounded-xl border-5 w-[60%] h-[60%] animate-pulse"
              />
            </div>
          </template>
        </template>
      </div>
    </div>
  </div>

  <Transition name="fade">
    <div v-if="gameState.isGameOver" class="fixed inset-0 shadow-2xl bg-black/75 flex items-center justify-center z-50">
      <NoMoves :restart-game="restartGame" />
    </div>
  </Transition>
</template>

<style scoped>
</style>