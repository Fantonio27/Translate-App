<script setup lang="ts">
  import { reactive, watch } from 'vue';
import Navbar from './components/Navbar.vue';
  import TextField from './components/TextField.vue';

  const translate = reactive<{
    translateFrom: string,
    translateTo: string
  }>({
    translateFrom:"Hello, how are you?",
    translateTo: "Bonjour, comment vas-tu?"
  });

  const languages = reactive<{
    firstLanguage: string,
    secondLanguage: string
  }>({
    firstLanguage: "English",
    secondLanguage: "French"
  })
  
  watch(languages, ()=>{
    // fetch('https://api.mymemory.translated.net/get?q=Hello,%20how%20are%20you?!&langpair=en|sp')
    // .then(response => response.json())
    // .then(data => {
    //   console.log(data)
    // })
    // .catch(error => {
    //   console.log(error)
    // })
  })

  const switchLanguage = () => {
    const {firstLanguage, secondLanguage} = languages

    languages.firstLanguage = secondLanguage;
    languages.secondLanguage = firstLanguage;
  }

</script>

<template>
  <div>
    <Navbar />

    <div class="container">
      <TextField 
        :translateTo="true" 
        :language="languages.firstLanguage" 
        :text="translate.translateFrom" 
        @changeLanguage="(lang)=>{languages.firstLanguage = lang}"
        :data="translate"
      />

      <TextField 
        :translateTo="false" 
        :language="languages.secondLanguage" 
        :text="translate.translateTo"
        @changeLanguage="(lang)=>{languages.secondLanguage = lang}"
        @switchLanguage="switchLanguage"
        :data="translate"
      />
    </div>
  </div>
</template>

<style scoped>
  .container{
    display: flex;
    justify-content: center;
    width: 90%;
    gap: 20px;
    margin: auto;
  }

  @media screen and (max-width: 1024px){
    .container{
      flex-direction: column;
      width: 80%;
      padding-bottom: 30px;
    }
  }

  @media screen and (max-width: 640px){
    .container{
      width: 95%;
    }
  }
</style>
