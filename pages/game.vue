<template>
<div class="flex flex-col flex-nowrap align-items-center game">
  <div class="overflow-hidden bg-white shadow sm:rounded-lg">
    <div class="border-b-2 border-gray-200 p-4">
      <img :src="require(`@/static/profile/${picture}`)" />
		</div>
    
    <div class="border-b-2 border-gray-200 p-4 flex justify-between">
      <div>Current score: {{ points }} / {{ total }}</div>
      <button 
        @click="goToNext"
        v-if="showNext !== 'remove'"
        class="text-sky-600 text-lg transition ease-in-out duration-200"
        :class="{ 'opacity-50' : showNext === 'disabled' }"
        :disabled="showNext === 'disabled'">Next question {{showNext}}</button>
		</div>
    
    <div class="grid grid-cols-2 gap-[2px] bg-gray-200">
      <GameButton :num="1" :name="answers[0]" @clicked="handleButtonClick" :state="answersState[1]" :disabled="disableButtons" />
      <GameButton :num="2" :name="answers[1]" @clicked="handleButtonClick" :state="answersState[2]" :disabled="disableButtons" />
      <GameButton :num="3" :name="answers[2]" @clicked="handleButtonClick" :state="answersState[3]" :disabled="disableButtons" />
      <GameButton :num="4" :name="answers[3]" @clicked="handleButtonClick" :state="answersState[4]" :disabled="disableButtons" />
    </div>
	</div>
  
    <div class="p-4">
      <nuxt-link to="/" class="text-sky-600">Return home</nuxt-link>
		</div>
</div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import GameButton from '~/components/GameButton.vue';
import senators from '~/data/senators.json';

type TShowNext = "disabled" | "remove" | "show";

let answers: any = ref([]);
let answersState: any = ref({
  "1": 'normal',
  "2": 'normal',
  "3": 'normal',
  "4": 'normal'
});
let currGoodAnswer = ref(0);
let disableButtons = ref(false);
let picture = ref('');
let points = ref(0);
let senatorsList: any[] = [];
let showNext: any = ref('disabled');
const senatorsRaw = [ ...senators.list ];
const senatorsToSplice = [ ...senators.list ];
let stage = ref(1);
const total = ref(10);

generateSenatorList();
fillDataWithSenator();
console.log('senatorsList', senatorsList);
console.log('answers', answers);

function fillDataWithSenator(index: number = 1) {
  const curr = senatorsList[index - 1];
  answers.value = [
    curr.answers[1],
    curr.answers[2],
    curr.answers[3],
    curr.answers[4]
  ]
  currGoodAnswer.value = curr.goodAnswer;
  picture.value = curr.image;
  console.log('currGoodAnswer', currGoodAnswer.value);
}

function generateSenatorList(count: number = 1) {
  if (count <= total.value) {
    const randIndex = Math.floor(Math.random() * senatorsToSplice.length);
    const goodAnswer = Math.ceil(Math.random() * 4);
    const filteredSenatorsBySex = senatorsRaw.filter(sen => sen.sex === senatorsToSplice[randIndex].sex && sen.name !== senatorsToSplice[randIndex].name);
    
    const item: any = {
      ...senatorsToSplice[randIndex],
      "goodAnswer": goodAnswer,
      "answers": {}
    }
    
    for (let i = 1; i <= 4; i++) {
      if (i !== goodAnswer) {
        const answerRandIndex = Math.floor(Math.random() * filteredSenatorsBySex.length);
        item.answers[i] = filteredSenatorsBySex[answerRandIndex].name;
        filteredSenatorsBySex.splice(answerRandIndex, 1);
      } else {
        item.answers[i] = item.name;
      }
    }
    
    senatorsList.push(item);
    senatorsToSplice.splice(randIndex, 1);
    
    generateSenatorList(count + 1);
  }
}

function handleButtonClick(num: number) {
  for (const key in answersState.value) {
    answersState.value[key] = 'disabled';
  }
    
  if(num === currGoodAnswer.value) {
    answersState.value[currGoodAnswer.value] = 'correct';
    points.value++;
  } else {
    answersState.value[currGoodAnswer.value] = 'highlight';
    answersState.value[num] = 'error';
  }
  
  if (showNext.value !== 'remove') {
    showNext.value = 'show';
  }
}

function goToNext() {
  stage.value = stage.value + 1;
  
  fillDataWithSenator(stage.value);
  resetButtons();
  
  if (stage.value === total.value) {
    showNext.value = 'remove';
  }
}

function resetButtons() {
  for (const key in answersState.value) {
    answersState.value[key] = 'normal';
  }
  
  disableButtons.value = false;
  showNext.value = 'disabled';
}

</script>

<style>
	.game {
		width: min(92%, 600px);
	}
</style>