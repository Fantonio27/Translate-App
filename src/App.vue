<script setup lang="ts">
  import { reactive } from 'vue';
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

  const reference: {
    English: string,
    French: string,
    Spanish: string
  }= {
    English: 'en',
    French: 'fr',
    Spanish: 'es'
  }
  
  const translation = async() => {
    const equal = languages.firstLanguage == languages.secondLanguage

    if(!equal){
      const response = await fetch(`https://api.mymemory.translated.net/get?q=${translate.translateFrom}&langpair=${reference[languages.firstLanguage]}|${reference[languages.secondLanguage]}`)
      const data = await response.json()

      translate.translateTo = equal? '' : data.matches[0].translation;
    }else{
      translate.translateTo = 'Please select two Distinct Languages'
    }
  
  }

  const switchLanguage = () => {
    const {firstLanguage, secondLanguage} = languages
    const {translateFrom, translateTo} = translate

    languages.firstLanguage = secondLanguage;
    languages.secondLanguage = firstLanguage;

    translate.translateFrom = translateTo;
    translate.translateTo = translateFrom;
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
        @translation="translation"
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
