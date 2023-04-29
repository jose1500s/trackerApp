<script setup>
import { ref, onMounted } from 'vue';
import axios from "axios";
import Dropdown from 'primevue/dropdown';
import TeamCard from './components/TeamCard.vue';

components: {
  Dropdown,
  TeamCard
}

const selectLeague = ref('');
const selectedTeam = ref('');
const equipos = ref([]);
const teamsToAdd = ref([]);

const leagues = [
  { name: 'Premier League', id: 39 },
  { name: 'La Liga', id: 140 },
  { name: 'Bundesliga', id: 78 },
  { name: 'Serie A', id: 135 },
  { name: 'Ligue 1', id: 61 },
  { name: 'Uefa Champions League', id: 2 },
];

const searchTeam = () => {
  const year = new Date().getFullYear();
  const yearReal = year - 1;
  const idLeague = selectLeague.value.id;


  const options = {
    method: 'GET',
    url: `https://v3.football.api-sports.io/teams?league=${idLeague}&season=${yearReal}`,
    params: {
      league: idLeague,
      season: yearReal
    },
    headers: {
      'x-rapidapi-key': '4f5fd403c8516718cfdedfe9ddf37efa'
    }
  };

  axios.request(options)
    .then(response => {
      equipos.value = response.data.response;
      console.log(equipos.value);
    })
    .catch(error => {
      console.error(error);
    });

}

const addTeam = () => {
  const team = { name: selectedTeam.value.team.name };
  teamsToAdd.value.push(team);
}


</script>

<template>
  <div class="text-center mt-28 w-1/2 m-auto">
    <div>
      <h1 class="bg-gradient-to-r from-green-300 via-blue-500 to-purple-600 bg-clip-text text-3xl 
              font-extrabold text-transparent sm:text-5xl p-3">
        Track Matches of Your Favorite Teams
      </h1>
      <h3 class="bg-gradient-to-r from-green-300 via-blue-500 to-purple-600 bg-clip-text text-2xl 
              font-extrabold text-transparent sm:text-2xl">
        Select and add to follow
      </h3>
    </div>
    <div class="w-full m-auto mt-10 flex justify-center items-center gap-10">
      <div class="league w-2/5">
     
        <Dropdown filter  v-model="selectLeague" :options="leagues" optionLabel="name" placeholder="Select a league" class="w-full" @change="searchTeam" />
      </div>

      <div class="team w-2/5">
        <Dropdown filter v-model="selectedTeam" :options="equipos" optionLabel="team.name" placeholder="Select a Team" class="w-full">
          <template #option="{ option }">
            <div class="flex items-center">
              <img :src="option.team.logo" :alt="option.team.name" class="w-5 h-5 mr-2">
              <div>{{ option.team.name }}</div>
            </div>
          </template>
        </Dropdown>


      </div>

      <div class="flex items-center">
        <button class="tooltip" @click="addTeam">
          <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24">
            <path fill="#2E86C1"
              d="M11 17h2v-4h4v-2h-4V7h-2v4H7v2h4v4Zm1 5q-2.075 0-3.9-.788t-3.175-2.137q-1.35-1.35-2.137-3.175T2 12q0-2.075.788-3.9t2.137-3.175q1.35-1.35 3.175-2.137T12 2q2.075 0 3.9.788t3.175 2.137q1.35 1.35 2.138 3.175T22 12q0 2.075-.788 3.9t-2.137 3.175q-1.35 1.35-3.175 2.138T12 22Zm0-2q3.35 0 5.675-2.325T20 12q0-3.35-2.325-5.675T12 4Q8.65 4 6.325 6.325T4 12q0 3.35 2.325 5.675T12 20Zm0-8Z" />
          </svg>
          <span class="tooltiptext">Add</span>
        </button>
      </div>

    </div>
  </div>

  <section>
    <div class="flex flex-wrap justify-center items-center gap-10 mt-10">
      <TeamCard v-for="(team, index) in teamsToAdd" :key="index" :name="team.name" />

    </div>
  </section>
</template>

<style>
:root {
  --primary-100: #2E86C1;
  --primary-200: #6ab6f4;
  --primary-300: #d6ffff;
  --accent-100: #FF5733;
  --accent-200: #fff3bf;
  --text-100: #333333;
  --text-200: #5c5c5c;
  --bg-100: #F5F5F5;
  --bg-200: #ebebeb;
  --bg-300: #c2c2c2;
}

body {
  background-image: linear-gradient(to top, #accbee 0%, #e7f0fd 100%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;

}

button {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1em;
  border: 0px solid transparent;
  background-color: rgba(100, 77, 237, 0.08);
  border-radius: 1.25em;
  transition: all 0.2s linear;
}

button:hover {
  box-shadow: 3.4px 2.5px 4.9px rgba(0, 0, 0, 0.025),
    8.6px 6.3px 12.4px rgba(0, 0, 0, 0.035),
    17.5px 12.8px 25.3px rgba(0, 0, 0, 0.045),
    36.1px 26.3px 52.2px rgba(0, 0, 0, 0.055),
    99px 72px 143px rgba(0, 0, 0, 0.08);
}

.tooltip {
  position: relative;
  display: inline-block;
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 4em;
  background-color: rgba(0, 0, 0, 0.253);
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  top: 25%;
  left: 110%;
}

.tooltip .tooltiptext::after {
  content: "";
  position: absolute;
  top: 50%;
  right: 100%;
  margin-top: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: transparent rgba(0, 0, 0, 0.253) transparent transparent;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}
</style>
