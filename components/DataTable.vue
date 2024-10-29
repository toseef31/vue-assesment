<template>
  <div>
    <div class="table-player-section">
      <div class="row flex-column-reverse flex-md-row">
        <div class="col-12 col-md-8">
          <div class="row">
            <div class="col-12 col-md-12">
              <div class="bg-white-10 rounded">
                <table class="w-100" style="background-color: transparent !important;">
                  <thead>
                    <tr>
                      <th v-for="header in headers" :key="header.value" class="p-2 px-3 text-white">
                        {{ header.text }}
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-if="paginatedPlayers.length === 0">
                      <td colspan="5" class="text-center text-white p-4 h6">No players found.</td>
                    </tr>
                    <tr
                      v-for="player in paginatedPlayers"
                      :key="player.operatorPlayerName"
                      class="text-white"
                      @click="selectPlayer(player)"
                      style="background-color: rgba(255, 255, 255, 0.1);"
                    >
                      <td class="p-2 px-3">{{ player.operatorPlayerName }}</td>
                      <td class="p-2 px-3">{{ player.team }}</td>
                      <td class="p-2 px-3">{{ player.operatorPosition }}</td>
                      <td class="p-2 px-3">{{ player.operatorSalary }}</td>
                      <td class="p-2 px-3">{{ player.fantasyPoints }}</td>
                    </tr>
                  </tbody>
                </table>
                <div class="p-2 d-flex justify-content-between align-items-center p-3">
                  <div class="page-drop d-flex align-items-center">
                    <label class="text-white">Page </label>
                    <select v-model="page" class="form-control form-select p-2 mx-2" style="width: 60px; height: 40px;">
                      <option v-for="n in pageCount" :key="n" :value="n">{{ n }}</option>
                    </select>
                  </div>
                  <div class="row-limit d-flex align-items-center">
                    <label class="text-white">Rows Per Page </label>
                    <select v-model="itemsPerPage" class="form-control form-select p-2 mx-2" style="width: 60px; height: 40px;">
                      <option value="8">8</option>
                      <option value="10">10</option>
                      <option value="20">15</option>
                      <option value="30">30</option>
                      <option value="50">50</option>
                    </select>
                  </div>
                  <div class="data-presentation">
                    <label class="text-white">{{ startItem }} - {{ endItem }} of {{ players ? players.length : 0 }}</label>
                  </div>
                  <div class="pagination-arrows d-flex align-items-center" style="width: 50px;">
                    <button @click="prevPage" :disabled="page === 1" class="btn p-1 mx-1 text-white"
                      style="width: 40px; height: 40px;">
                      <i class="fas fa-angle-left"></i>
                    </button>
                    <button @click="nextPage" :disabled="page === pageCount" class="btn p-1 mx-1 text-white"
                      style="width: 40px; height: 40px;">
                      <i class="fas fa-angle-right"></i>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-4">
          <div v-if="selectedPlayer" class="bg-white-10 rounded">
            <div class="player-image text-center pt-5">
              <img src="@/assets/Images/player1.png" class="mt-4" width="80%">
            </div>
            <div class="player-details pb-1 rounded bg-card-player">
              <div class="text-center player-name">{{ selectedPlayer.operatorPlayerName }}</div>
              <div class="text-center player-rank">{{ selectedPlayer.fantasyPoints || 'N/A' }}</div>
              <div class="text-center player-subtitle pb-5 mb-5">{{ selectedPlayer.team }}</div>
            </div>
          </div>
          <div v-else class="text-center text-white mt-4">Select a player to view details</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    players: {
      type: Array,
      default: () => [] // Fallback to an empty array
    }
  },
  data() {
    return {
      page: 1,
      itemsPerPage: 10,
      selectedPlayer: null, // Holds the currently selected player's data
      defaultImage: '/assets/Images/player1.png', // Default player image
      headers: [
        { text: "Name", value: "name" },
        { text: "Team", value: "team" },
        { text: "Position", value: "position" },
        { text: "Salary", value: "salary", align: "end" },
        { text: "Points", value: "points", align: "end" },
      ]
    };
  },
  computed: {
    paginatedPlayers() {
      if (!this.players || !Array.isArray(this.players) || this.players.length === 0) return []; // Ensure safety
      const start = (this.page - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      console.log(this.players);
      return this.players.slice(start, end);
    },
    pageCount() {
      if (!this.players || !Array.isArray(this.players)) return 1; // At least one page when empty
      return Math.ceil(this.players.length / this.itemsPerPage);
    },
    startItem() {
      if (!this.players || !Array.isArray(this.players) || this.players.length === 0) return 0; // Adjust as needed
      return (this.page - 1) * this.itemsPerPage + 1;
    },
    endItem() {
      if (!this.players || !Array.isArray(this.players) || this.players.length === 0) return 0; // Adjust as needed
      return Math.min(this.page * this.itemsPerPage, this.players.length);
    }
  },
  methods: {
    nextPage() {
      if (this.page < this.pageCount) this.page++;
    },
    prevPage() {
      if (this.page > 1) this.page--;
    },
    selectPlayer(player) {
      this.selectedPlayer = player;
    }
  },
  watch: {
    itemsPerPage() {
      this.page = 1; // Reset to page 1 if items per page changes
    }
  }
};
</script>
