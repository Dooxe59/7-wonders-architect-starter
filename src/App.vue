<template>
  <label for="number-player-select">Nombre de joueurs: </label>
  <select v-model="numberOfPlayer" id="number-player-select">
    <option v-for="n in 6" :key="n" :value="n + 1">{{n + 1}}</option>
  </select>

  <div v-for="m in numberOfPlayer" :key="m">
    <label :for="`player-name-${m}`">{{ `Joueur ${m}` }}: </label>
    <input :id="`player-name-${m}`" v-model="playerNames[m - 1]" />
  </div>

  <button :disabled="!isValidForm" @click="affectWonders">Définir les merveilles</button>

  <div v-if="affectedWonders">
      <div  
        v-for="result in affectedWonders" 
        :key="`${result.playerName}_${result.wonder}`">
        {{ result.playerName }} va jouer avec <b>{{ result.wonder }}</b>
      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      numberOfPlayer: 2,
      playerNames: [],
      wondersNames: ['Babylone', 'Gizeh', 'Alexandrie', 'Olympie', 'Rhodes', 'Halicarnasse', 'Ephése'],
      affectedWonders: []
    };
  },
  computed: {
    isValidForm() {
      return this.playerNames.length === this.numberOfPlayer 
        && this.playerNames.every(playerName => {
          return playerName?.trim()?.length > 0 && playerName !== null
        })
    }
  },
  methods: {
    affectWonders() {
      this.affectedWonders = []
      const wondersNamesCopied = [...this.wondersNames]
      this.playerNames.forEach(playerName => {
        const idx = Math.floor(Math.random() * wondersNamesCopied.length)
        const wonder = wondersNamesCopied[idx];
        this.affectedWonders.push({
          playerName,
          wonder
        })
        wondersNamesCopied.splice(idx, 1)
      })
    }
  }
}
</script>

<style>
#app {

}
</style>
