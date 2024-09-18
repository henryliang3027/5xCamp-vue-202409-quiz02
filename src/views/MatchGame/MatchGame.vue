<script setup>
import { ref } from "vue";
import Card from "./Card.vue";

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

const cards = ref([]);
const openedCard = ref([]);

// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map((i) => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map((d) => (d % 8) + 1);
  openedCard.value = [];
};

const checkWin = () => {
  const remainingCards = cards.value.filter((card) => card > 0);

  if (remainingCards.length == 0) {
    alert("恭喜破關，再來一局？");
    gameInit();
  }
};

const cardClickHandler = (idx) => {
  // 已開啟兩張卡片就不能再開啟其它張, 同時最多開啟兩張卡片
  if (openedCard.value.length >= 2) {
    console.log("true");
    return;
  }

  openedCard.value.push(idx);
  if (openedCard.value.length == 2) {
    const [firstIdx, secondIdx] = openedCard.value;
    console.log(firstIdx, secondIdx);
    if (cards.value[firstIdx] == cards.value[secondIdx]) {
      window.setTimeout(() => {
        cards.value[firstIdx] = 0;
        cards.value[secondIdx] = 0;
        openedCard.value = [];
        checkWin();
      }, 1000);
    } else {
      // 一秒後將 openedCard 清空 (牌面覆蓋回去)
      window.setTimeout(() => {
        openedCard.value = [];
      }, 1000);
    }
  }
};
</script>

<template>
  <div
    class="bg-emerald-900 min-h-screen w-full top-0 left-0 z-10 overflow-auto"
  >
    <div class="my-10 text-white text-center">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700"
      >
        開始
      </button>
    </div>

    <div
      class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4"
    >
      <Card
        v-for="(n, idx) in cards"
        :idx="idx"
        :n="n"
        :isOpen="openedCard.includes(idx)"
        @cardClicked="cardClickHandler(idx)"
      >
      </Card>
    </div>
  </div>
</template>

<!-- <style scoped src="./MatchGame.css"></style> -->
