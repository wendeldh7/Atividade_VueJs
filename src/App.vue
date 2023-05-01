<script setup>
  import { reactive, watch } from 'vue';

  const estado = reactive({
    el: '#app',
    data: {
      playerLife: 100,
      enemyLife: 100,
      running: true,
      log: [],
    },
    computed: {
      
    },
    methods:{

    },
  })


  const hasResult = () => {
        return estado.data.playerLife == 0 || estado.data.enemyLife == 0
  }

  const start = () => {
    estado.data.running = true
    estado.data.playerLife = 100
    estado.data.enemyLife = 100
    console.log('Clicou')
    estado.data.log - []
  }

  const attack = (especial) => {
    hurt( 7, 12, false, 'Inimigo', 'Jogador', 'enemy')
    hurtEnemy (5, 10, especial , 'Jogador', 'inimigo', 'player' )
  }

  const hurt = ( min, max, especial, source, target, cls) => {
    const plus = especial ? 5 : 0
    const hurt = randomPower(min + plus, max + plus)
    estado.data.playerLife = Math.max(estado.data.playerLife - hurt , 0)
    logRegister(`${source} atingiu ${target} com ${hurt}.`, cls)
  }

  const hurtEnemy = ( min, max, especial, source, target, cls) => {
    const plus = especial ? 5 : 0
    const hurt = randomPower(min + plus, max + plus)
    estado.data.enemyLife = Math.max(estado.data.enemyLife - hurt , 0)
    logRegister(`${source} atingiu ${target} com ${hurt}.`, cls)
  }

  const randomPower = (min, max) => {
    const value = Math.random() * (max - min) + min
    return Math.round(value)
  }

  const heal = () => {
    if(estado.data.playerLife < 90){
      estado.data.playerLife += 10
    } else {
      estado.data.playerLife += 0
    }
    logRegister("Jogador se curou em 10% de vida perdida", 'player')
  }

  const logRegister = (text, cls) => {
    estado.data.log.unshift({text , cls})
  }
</script>

<template>
    <div id="app">
      <div class="panel scores">
        <div class="score">
          <h1>Jogador</h1>
          <div class="life-bar">
            <div class="life" :class="{danger: estado.data.playerLife < 20}" :style="{width: estado.data.playerLife + '%'}">

            </div>
          </div>
          <div>{{ estado.data.playerLife }}%</div>
        </div>
        <div class="score">
          <h1>Enemy</h1>
          <div class="life-bar">
            <div class="life" :class="{danger: estado.data.enemyLife < 20}" :style="{width: estado.data.enemyLife + '%'}">

            </div>
          </div>
          <div>{{ estado.data.enemyLife }}%</div>
        </div>
      </div>
      <div v-if="hasResult()" class="panel result">
        <div v-if="estado.data.enemyLife == 0" class="win">Você ganhou!!</div>
        <div v-else class="lose">Você perdeu..</div>
      </div>
      <div class="panel btns">
        <template v-if="estado.data.running">
          <button class="btn attack" @click="attack(false)">Ataque</button>
          <button class="btn special" @click="attack(true)">Ataque Especial</button>
          <button class="btn heal" @click="heal">Curar</button>
          <button class="btn giveUp" @click="estado.data.running = false">Reiniciar</button>
        </template>
        <button v-else @click="start()" class="btn newGame">Iniciar</button>
      </div>
      <div v-if="estado.data.log.length" class="panel logs">
        <ul>
          <li v-for="logs in estado.data.log" :class="logs.cls" class="log">
            {{ logs.text }}
          </li>
        </ul>  
      </div>
    </div>
</template>

<style scoped>
  #app {
    display: flex;
    flex-direction: column;
  }

  .panel{
    margin: 10px;
    padding: 20px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
  }

  .scores{
    display: flex;

  }

  .score{
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .score h1{
    font-weight: 300;
    font-size: 1.6rem;
  }

  .life-bar{
    width: 80%;
    height: 20px;
    border: 1px solid #aaa;
  }

  .life-bar .life{
    display: flex;
    justify-content: center;
    height: 100%;
    background-color: green;
  }

  .life-bar .life .danger{
    background-color: red;
  }

  .result{
    display: flex;
    justify-content: center;
    font-size: 1.3rem;
    font-weight: 700;
  }

  .result .win{
    color: green;
  }

  .result .lose{
    color: red;
  }

  .btns{
    display: flex;
    justify-content: center;
    gap: 15px;
  }

  .btn{
    padding: 5px 10px;
    border-radius: 5px;
    text-transform: uppercase;
    font-size: 1rem;
    border: 1px solid transparent;
    cursor: pointer;
  }

  .newGame{
    background-color: rgb(19, 127, 216);
    color: white;
  }

  .giveUp{
    background-color: orange;
    color: whitesmoke;
  }

  .heal{
    background-color: greenyellow;
    color: white;
  }

  .special{
    background-color: blueviolet;
    color: whitesmoke;
  }

  .attack{
    background-color: rgb(190, 47, 47);
    color: white;
  }

  .logs ul{
    display: flex;
    flex-direction: column;
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .logs ul li{
    display: flex;
    justify-content: center;
    margin: 4px 0px;
    padding: 3px 0px;
    font-weight: 700;
    font-size: 1.1rem;
    text-transform: uppercase;
    color: white;
    border-radius: 3px;
  }

  .player{
    background-color: #4253afaa;
  }

  .enemy{
    background-color: #e51c23aa;
  }



</style>
