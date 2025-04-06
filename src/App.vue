<template>
  <div class="container">
    <h1>가위바위보 게임</h1>
    
    <div class="score-board">
      <div class="score">
        <h2>당신의 점수: {{ playerScore }}</h2>
        <h2>컴퓨터 점수: {{ computerScore }}</h2>
      </div>
    </div>

    <div class="game-area">
      <div class="choices">
        <button 
          v-for="choice in choices" 
          :key="choice"
          @click="play(choice)"
          :class="{ 'selected': playerChoice === choice }"
        >
          {{ getEmoji(choice) }}
        </button>
      </div>

      <div class="result" v-if="result">
        <div class="choices-display">
          <div class="player-choice">
            <h3>당신의 선택</h3>
            <span class="big-emoji">{{ getEmoji(playerChoice) }}</span>
          </div>
          <div class="vs">VS</div>
          <div class="computer-choice">
            <h3>컴퓨터의 선택</h3>
            <span class="big-emoji">{{ getEmoji(computerChoice) }}</span>
          </div>
        </div>
        <h2 :class="resultClass">{{ result }}</h2>
      </div>

      <button class="reset-btn" @click="resetGame" v-if="result">다시하기</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const choices = ['rock', 'paper', 'scissors']
const playerScore = ref(0)
const computerScore = ref(0)
const playerChoice = ref(null)
const computerChoice = ref(null)
const result = ref('')

const resultClass = computed(() => {
  switch (result.value) {
    case '승리!': return 'result-win'
    case '패배!': return 'result-lose'
    case '무승부!': return 'result-draw'
    default: return ''
  }
})

const getEmoji = (choice) => {
  const emojis = {
    rock: '✊',
    paper: '✋',
    scissors: '✌️'
  }
  return emojis[choice]
}

const getComputerChoice = () => {
  const randomIndex = Math.floor(Math.random() * choices.length)
  return choices[randomIndex]
}

const determineWinner = (player, computer) => {
  if (player === computer) return '무승부!'
  
  const winConditions = {
    rock: 'scissors',
    paper: 'rock',
    scissors: 'paper'
  }
  
  return winConditions[player] === computer ? '승리!' : '패배!'
}

const play = (choice) => {
  playerChoice.value = choice
  computerChoice.value = getComputerChoice()
  
  const gameResult = determineWinner(playerChoice.value, computerChoice.value)
  result.value = gameResult
  
  if (gameResult === '승리!') {
    playerScore.value++
  } else if (gameResult === '패배!') {
    computerScore.value++
  }
}

const resetGame = () => {
  playerChoice.value = null
  computerChoice.value = null
  result.value = ''
}
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

.score-board {
  margin: 2rem 0;
  padding: 1rem;
  background-color: #f5f5f5;
  border-radius: 8px;
}

.score {
  display: flex;
  justify-content: space-around;
}

.choices {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-bottom: 2rem;
}

button {
  font-size: 2rem;
  padding: 1rem 2rem;
  border: none;
  background-color: #fff;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.2s;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

button:hover {
  transform: scale(1.1);
}

.selected {
  background-color: #e0e0e0;
}

.choices-display {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  margin: 2rem 0;
}

.big-emoji {
  font-size: 4rem;
}

.vs {
  font-size: 1.5rem;
  font-weight: bold;
}

.result {
  margin: 2rem 0;
  font-size: 1.5rem;
}

.result-win {
  color: #4caf50;
}

.result-lose {
  color: #f44336;
}

.result-draw {
  color: #2196f3;
}

.reset-btn {
  font-size: 1.2rem;
  padding: 0.5rem 1rem;
  background-color: #2196f3;
  color: white;
}

.reset-btn:hover {
  background-color: #1976d2;
}
</style>