<template>
  <div class="wrapper">
    <h3 class="header">
      NBA PLAYER'S HEIGHT SEARCH
    </h3>
    <PairSearchBar :searchTerm="searchTerm" @termChange="handleTermChange"/>
    <PairTable :items="selectedPairs"/>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  async fetch () {
    fetch('https://mach-eight.uc.r.appspot.com/')
      .then(response => response.json())
      .then(data => this.players = data.values)
  },
  data: () => ({
    searchTerm: undefined,
    players: [],
    selectedPairs: []
  }),
  methods: {
    handleTermChange (event) {
      if (parseInt(event) > 0 && this.players.length > 0) {
        this.selectedPairs = []
        this.searchTerm = parseInt(event)
        this.lookForPairs()
      }
    },
    lookForPairs (x = 0, y = 1) {
      for (let i = 0; i < (this.players.length - 2); i++) {
        const secArray = this.players
                            .slice(i + 1)
                            .filter(p => ( this.searchTerm - parseInt(this.players[i].h_in)) === parseInt(p.h_in) )
                            .map(p => ({ first: this.getPlayerFullName(this.players[i]), second: this.getPlayerFullName(p) }))
        this.selectedPairs = [...this.selectedPairs, ...secArray]
      }
    },
    getPlayerFullName(player) {
      return `${player.first_name} ${player.last_name}`
    }
  }
}
</script>

<style scoped>
  .wrapper {
    padding: 10px;
  }
  .header {
    text-align: center;
  }
</style>
