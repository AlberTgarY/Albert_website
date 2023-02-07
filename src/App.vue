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
  const url1 = 'https://github.com/AlberTgarY/SensatUrban_albert'
  // Send an async HTTP Get request to the url

    
  // This is the structure of the player data we recieve
  interface PlayerData {
    name: string;
  }

  // Send an async HTTP Get request to the url
    AxiosInstance.get(url1)
    .then( // Once we have data returned ...
      response => {
        const html = response.data; // Get the HTML from the HTTP request
        const $ = cheerio.load(html); // Load the HTML string into cheerio
        const statsTable: Cheerio<any> = $(".Details-content--hidden-not-important.js-navigation-container.js-active-navigation-container.d-md-block"); // Parse the HTML and extract just whatever code contains .statsTableContainer and has tr inside
        const statsTable1: Cheerio<any> = $(".js-active-navigation-container.d-md-block > div");
        
        const topScorers: PlayerData[] = [];
        // for (let i = 0; i < statsTable1.length ; i++) {
        //   const name: string = $(elem).find('div:nth-child('+'3'+') > div.flex-auto.min-width-0.col-md-2.mr-3 > span').text(); // Parse the name
        //   topScorers.push({
        //     name,
        //   })
        //   console.log ("Block statement execution no." + i);
        // }
        statsTable.each((i, elem) => {
          for (let i = 0; i <= statsTable1.length; i++) {
            const name: string = $(elem).find('div:nth-child('+i.toString()+') > div.flex-auto.min-width-0.col-md-2.mr-3 > span').text(); // Parse the name
            topScorers.push({
              name,
            })
          }
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
