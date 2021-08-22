<template>
<div class="container">
  <div class="row">
    <h1 class="mt-2">Words Translate</h1>
    <input
      type="text"
      class="form-control bg-dark text-light rounded-0 border-0 my-4"
      placeholder="Search Coin"
      @keyup="translateWord()"
      v-model="word"
    />
    <table class="table table-dark">
      <thead>
        <tr>
          <th class="m-2 text-uppercase" v-for="title in titles" :key="title">
            {{ title }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(translation) in translations" :key="translation">
          <td>
            {{translation.from}}
          </td>
          <td>
            {{translation.to}}
          </td>
          <td>
            {{translation.toType}}
          </td>
          <td>
            {{translation.example.from[0]}}
          </td>
          <td>
            {{translation.example.to[0]}}
          </td>        
        </tr>
      </tbody>
    </table>    
  </div>
</div>
</template>

<script>
//<img alt="Vue logo" src="./assets/logo.png">
//<HelloWorld msg="Welcome to Your Vue.js App"/>
//import HelloWorld from './components/HelloWorld.vue'
import  wr  from "wordreference-api";

export default {
  name: 'App',
  components: {
    //HelloWorld
  },
  data() {
    return {
      word: "hello",
      startLenguage: "en",
      endLenguage: "es",
      titles: [`English`, `Spanish`, `type`, `Example`, `Ejemplo`],
      translations: [],
    };
  },
  async mounted() {
    this.translateWord();
  },
  methods: {
    async translateWord () {
      if(this.word !== ""){
        try {
          //console.log(`${this.word} ${this.startLenguage} ${this.endLenguage}`);
          const jsonWord = await wr(this.word, this.startLenguage, this.endLenguage);
          console.log(jsonWord.translations[0].translations);
          this.translations = jsonWord.translations[0].translations;
        } catch (e) {
          console.log("Error to get word translation: " + e);
        }          
      }      
    },
  },  
}
/*
let content = document.getElementById('textArea');

const pasteToClickBoard = async () => {
  try {
    const paste = await navigator.clipboard.readText();
    content.innerHTML = paste;
    console.log(paste);
  } catch (err) {
    console.log('Something went wrong', err);
  }
}

document.addEventListener('keydown', (event) => {
  var name = event.key;
  var code = event.code;
  
  console.log(`Key pressed ${name} \r\n Key code value: ${code}`);
  if(code === "ControlRight") pasteToClickBoard();
}, false);
*/
</script>

<style>
</style>
