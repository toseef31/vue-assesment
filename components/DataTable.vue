<template>
  <div>
<table class="w-100" style="background-color: transparent !important;">
  <thead>
    <tr>
      <th
        v-for="header in headers"
        :key="header.value"
        class="p-2 px-3 text-white"
      >
        {{ header.text }}
      </th>
    </tr>
  </thead>
  <tbody>
    <tr
      v-for="player in players"
      :key="player.name"
      class="text-white"
      style="background-color: rgba(255, 255, 255, 0.1);"
    >
      <td class="p-2 px-3">{{ player.name }}</td>
      <td class="p-2 px-3">{{ player.team }}</td>
      <td class="p-2 px-3">{{ player.position }}</td>  
      <td class="p-2 px-3">{{ player.salary }}</td>
      <td class="p-2 px-3">{{ player.points }}</td>
    </tr>
  </tbody>
</table>
<div class="p-2 d-flex justify-content-between align-items-center p-3">
  <div class="page-drop d-flex align-items-center">
      <label class="text-white">Page </label>
      <select class="form-control form-select p-2 mx-2" style="width: 60px; height: 40px;">
        <option>1</option>
        <option>2</option>
        <option>3</option>
      </select>
  </div>
  <div class="row-limit d-flex align-items-center">
      <label class="text-white">Rows Per Page </label>
      <select class="form-control form-select p-2 mx-2" style="width: 60px; height: 40px;">
        <option>8</option>
        <option>10</option>
        <option>15</option>
      </select>
  </div>
  <div class="data-presentation">
      <label class="text-white">1 - 8 of 200</label>
  </div>
  <div class="pagination-arrows d-flex align-items-center" style="width: 50px;">
      <button class="btn p-1 mx-1 text-white" style="width: 40px; height: 40px;">
        <i class="fas fa-angle-left"></i>
      </button>
      <button class="btn p-1 mx-1 text-white" style="width: 40px; height: 40px;">
        <i class="fas fa-angle-right"></i>
      </button>
  </div>
</div>
</div>
</template>
<script>
export default {
  data() {
    return {
      page: 1,
      itemsPerPage: 8,
      headers: [
        { text: "Name", value: "name" },
        { text: "Team", value: "team" },
        { text: "Position", value: "position" },
        { text: "Salary", value: "salary", align: "end" },
        { text: "Points", value: "points", align: "end" },
      ],
      players: [
        { name: "Tom Brady", team: "TB", position: "QB", salary: "$11,200", points: 23 },
        { name: "Patrick Mahomes", team: "KC", position: "QB", salary: "$8,800", points: 23 },
        { name: "Aaron Rodgers", team: "GB", position: "QB", salary: "$9,500", points: 20 },
        { name: "Derrick Henry", team: "TEN", position: "RB", salary: "$10,000", points: 19 },
        { name: "Davante Adams", team: "GB", position: "WR", salary: "$8,700", points: 22 },
        { name: "Travis Kelce", team: "KC", position: "TE", salary: "$7,600", points: 18 },
        { name: "Alvin Kamara", team: "NO", position: "RB", salary: "$8,500", points: 21 },
        { name: "Stefon Diggs", team: "BUF", position: "WR", salary: "$8,200", points: 19 },
        { name: "Tom Brady", team: "TB", position: "QB", salary: "$11,200", points: 23 },
        { name: "Patrick Mahomes", team: "KC", position: "QB", salary: "$8,800", points: 23 },
        { name: "Aaron Rodgers", team: "GB", position: "QB", salary: "$9,500", points: 20 },
        // Add more player data here as needed
      ]
    };
  },
  methods: {
    onPageChange(newPage) {
      this.page = newPage;
    },
    selectPlayer(player) {
      // Handle player selection logic
      console.log("Selected Player:", player);
    }
  }
};
</script>

<style scoped>
.v-data-table {
  background-color: #333;
  color: white;
}

.v-data-table th {
  background-color: #444;
  color: #fff;
}

.v-data-table td {
  color: #eee;
}

.v-btn {
  text-transform: none;
}
</style>