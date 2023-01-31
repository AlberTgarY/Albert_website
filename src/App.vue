<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import {ref} from 'vue'
import axios from 'axios'
import cheerio, { type Cheerio } from 'cheerio'
import { createDOMCompilerError } from '@vue/compiler-dom'

const restaurantList = ref<String[]>([])
const newRestaurantName = ref('');
const AxiosInstance = axios.create(); // Create a new Axios Instance

function log() {
  const url = 'https://www.premierleague.com/stats/top/players/goals?se=-1&cl=-1&iso=-1&po=-1?se=-1';
  // Send an async HTTP Get request to the url

    
  // This is the structure of the player data we recieve
  interface PlayerData {
    rank: number; // 1 - 20 rank
    name: string;
    nationality: string;
    goals: number;
  }

  // Send an async HTTP Get request to the url
  AxiosInstance.get(url)
    .then( // Once we have data returned ...
      response => {
        const html = response.data; // Get the HTML from the HTTP request
        const $ = cheerio.load(html); // Load the HTML string into cheerio
        const statsTable: Cheerio<any> = $('.statsTableContainer > tr'); // Parse the HTML and extract just whatever code contains .statsTableContainer and has tr inside
        const topScorers: PlayerData[] = [];

        statsTable.each((i, elem) => {
          const rank: number = parseInt($(elem).find('.rank > strong').text()); // Parse the rank
          const name: string = $(elem).find('.playerName > strong').text(); // Parse the name
          const nationality: string = $(elem).find('.playerCountry').text(); // Parse the country
          const goals: number = parseInt($(elem).find('.mainStat').text()); // Parse the number of goals
          topScorers.push({
            rank,
            name,
            nationality,
            goals
          })
        })

        console.log(topScorers);
      }
    )
    .catch(console.error); // Error handling
};



</script>

<template>
  <header>
    <h1>My Website2</h1>
  </header>
  <main>
    <form @submit.prevent="log">
      <input id="restaurant-name"
      v-model="newRestaurantName" type="text" />
      <button type="submit">Click NE</button>
    </form>
    <ul>
      <li v-for="rest in restaurantList" >
        {{ rest }}
      </li>
    </ul>
    
  </main>
</template>


<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
