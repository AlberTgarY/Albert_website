<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import {ref} from 'vue'
import axios from 'axios'
import cheerio, { type Cheerio } from 'cheerio'
import { createDOMCompilerError } from '@vue/compiler-dom'

interface Repo {
    name: string
    link: string
    files: Array<[string, string]>
}
const repoCache: string[] = [];
const header = 'https://github.com';
const RepoContent = ref<Repo[]>([]);
const url = ref('');
const AxiosInstance = axios.create(); // Create a new Axios Instance

function log() {
  // Send an async HTTP Get request to the url
  // const url = 'https://github.com/AlberTgarY/SensatUrban_albert'
  console.log(url);
  // RepoContent = ref<Repo[]>([]);
  // Send an async HTTP Get request to the url
  
  
  AxiosInstance.get(url.value)
    .then( 
      response => {
        
        const html = response.data; // Get the HTML from the HTTP request
        const $ = cheerio.load(html); // Load the HTML string into cheerio
        const statsTable: Cheerio<any> = $(".Details-content--hidden-not-important.js-navigation-container.js-active-navigation-container.d-md-block"); // Parse the HTML and extract just whatever code contains .statsTableContainer and has tr inside
        const fileDiv: Cheerio<any> = $(".js-active-navigation-container.d-md-block > div");
        const repoNameDiv: Cheerio<any> = $(".px-md-4.px-lg-5");

        let found:boolean = false;
        let curRepo = "";
        let repoLink = "";
        let fileList: Array<[string, string]> = [];
        // parse the repo name 
        repoNameDiv.each((_, elem) => {
          const temp =  $(elem).find('div > div > strong').text();
          if(temp!=""){
            curRepo = temp;
            // check if url has already exist
            found = repoCache.includes(curRepo);
            repoLink = url.value;
            if(!found){
              repoCache.push(curRepo);
            } 
          }
        })
        // Parse the files name
        statsTable.each((_, elem) => {
          for (let i = 0; i <= fileDiv.length; i++) {
            const targrt: any = $(elem).find('div:nth-child('+i.toString()+') > div.flex-auto.min-width-0.col-md-2.mr-3 > span > a');
            const curFile: string = targrt.text(); 
            const curFile_url: string = targrt.attr('href'); 
            if (curFile!=""){
              const temp:[string, string] = [curFile, curFile_url];
              fileList.push(temp)
            }
          }
        })
        console.log(found)
        if (!found){
          console.log("push")
          RepoContent.value.push({
            name : curRepo,
            link : repoLink,
            files: fileList
          })
        }
        else{
          throw new Error("The url "+ repoLink +" has already exist!");
        }
      }
      
    )
    .catch(console.error); // Error handling
};


</script>

<template>
  <header>
    <h1>Albert Website</h1>
  </header>
  <main>
    <form @submit.prevent="log" class="row g-3">
      <div class="col-auto">
        <input v-model="url" type="text" class="form-control" placeholder="github repository url" aria-label="Username" aria-describedby="basic-addon1">
      </div>
      <div class="col-auto">
        <button  class="btn btn-outline-primary" type="submit">Click ME</button>
      </div>
    </form>
    <ul class="list-group">
      <li class="list-group-item" v-for="repo in RepoContent" >
        <!-- repository name -->
        <a v-bind:href="repo.link">
        {{ repo.name }}
        </a>
        <li class="list-group-item" v-for="f in repo.files" >
          <!-- file names and hyperlink -->
          <a v-bind:href="header+f[1]">
          {{ f[0] }}
          </a>
        </li>

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
