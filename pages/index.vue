<template>
  <div>
    <input
      type="text"
      v-model="searchTerm"
      @input="termChange">
    <table>
      <thead>
        <tr>
          <td>1st Player</td>
          <td>2nd Player</td>
        </tr>
      </thead>
      <tbody>
        <tr :key="key" v-for="(pair, key) in selectedPairs">
          <td>{{getPlayerFullName(pair.first)}}</td>
          <td>{{getPlayerFullName(pair.second)}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  async fetch () {
    fetch('https://mach-eight.uc.r.appspot.com/')
      .then(response => response.json())
      .then(data => {
        this.players = data.values.slice(0, 5);
      });
  },
  data: () => ({
    searchTerm: 0,
    players: [],
    selectedPairs: []
  }),
  methods: {
    termChange (event) {
      if (parseInt(event.target.value) > 0 && this.players.length > 0) {
        this.searchTerm = parseInt(event.target.value)
        this.lookForPairs()
      }
    },
    lookForPairs (x = 0, y = 1, acum = []) {
      const sum = (parseInt(this.players[x].h_in) + parseInt(this.players[y].h_in));
      
      if (x === (this.players.length - 2)) {
        return;
      } else if (y < (this.players.length - 1)) {
        if (sum === this.searchTerm) this.selectedPairs.push({ first: this.players[x], second: this.players[y] })
        this.lookForPairs(x, y + 1);
      } else if (y === (this.players.length - 1)) {
        if (sum === this.searchTerm) this.selectedPairs.push({ first: this.players[x], second: this.players[y] })
        this.lookForPairs(x + 1, x + 2)
      } else {
        return;
      }
    },
    getPlayerFullName(player) {
      return `${player.first_name} ${player.last_name}`
    }
  }
}
</script>
