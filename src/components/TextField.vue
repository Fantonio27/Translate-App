<template>
    <div class="box-container" :style="{'background-color': translateTo? '#212936cc' : '#121826cc'}">
      <section class="language-box">
        <div>
            <p v-if="translateTo" style="padding: 0px 15px;">Detect Language</p>
            <ul class="language-lists">
                <li 
                    v-for="(list,index) in ['English', 'French', 'Spanish']" 
                    :class="{active:ActiveLang(list)}" 
                    @click="emit('changeLanguage', list)"
                >
                    {{ list }}
                    <span v-if="index == 2" style="margin-top: 5px;">
                        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M12 6L8 10L4 6" :stroke="ActiveLang(list)? '#F9FAFB': '#6C727F'" stroke-width="2"/>
                        </svg>
                    </span>
                </li>
            </ul>
        </div>

        <button class="icon-button switch" v-if="!translateTo" @click="emit('switchLanguage')">
            <img src="../assets/Horizontal_top_left_main.svg" alt="sound-icon">
        </button>
      </section>

      <div>
        <textarea 
            id="textField" 
            rows="6" 
            class="textarea" 
            resize="false" 
            :disabled="!translateTo"
            v-model="data[translateTo? 'translateFrom': 'translateTo']">{{ text }}
        </textarea>
        <p class="number-text">{{translateTo? '19/500' : '&nbsp;'}}</p>
      </div>

      <section>
        <div>
            <button class="icon-button" style="margin-right: 10px;" @click="speak">
                <img src="../assets/sound_max_fill.svg" alt="sound-icon">
            </button>
            <button class="icon-button">
                <img src="../assets/Copy.svg" alt="copy-icon" @click="copyClipboard">
            </button>
        </div>

        <button class="translate-button" v-if="translateTo" @click.passive="emit('translation')">
            <img src="../assets/Sort_alfa.svg"> 
            <span>Translate</span>
        </button>
      </section>
    </div>
</template>

<script setup lang="ts">
    import { computed } from 'vue';

    type dataType = {
        translateFrom: string,
        translateTo: string
    }

    const props = defineProps<{translateTo: boolean, language:string, text:string, data: dataType}>();
    const emit = defineEmits(['changeLanguage', 'switchLanguage', 'translation']);

    const ActiveLang = computed(()=> (lang:string)=> {
        return props.language == lang
    })

    const copyClipboard = () => {
        navigator.clipboard.writeText(props.text)
        alert(`Text Copied: ${props.text}`)
    } 

    const speak = () => {
        const utterance = new SpeechSynthesisUtterance(props.text)
        window.speechSynthesis.speak(utterance)
    } 
</script>

<style scoped>
    .box-container{
        border: 1px solid #4D5562;
        border-radius: 25px;
        width: 500px;
        padding: 10px 25px 20px;
    }

    .box-container > section {
        display: flex;
        gap: 20px;
        justify-content: space-between; 
        /* align-items: flex-end;   */
        align-items: center; 
    }

    .box-container > section:last-child{
        height: 50px;
    }

    .language-box{
        border-bottom: 1px solid #394150;
        padding: 10px 0px;
        font-weight: 600;
        color: #6B7280;
    }

    .language-box > div, .language-lists{
        display: flex;
        gap: 20px;
        align-items: center;
    }

    .language-lists{
        gap: 10px;
    }

    li{
        padding: 8px 15px;
        border-radius: 10px;
        list-style-type: none;
        font-size: 0.9rem;
        transition: 0.3s linear;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 5px;
    }

    li path{
        transition: 0.3s linear;
    }

    li:hover{
        color: #F9FAFB;
    }

    li:hover path{
        stroke: #F9FAFB;
    }
    
    .icon-button img{
        width: 20px;
        height: 20px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);

    }

    .icon-button{
        background-color: transparent;
        border: 2px solid #4D5562;
        border-radius: 12px; 
        width: 35px;
        height:35px;
        position: relative;
        cursor: pointer;
        transition: 0.3s;
    }

    .icon-button:hover{
        border: 2px solid #93979c;
    }

    .translate-button{
        font-weight: 550;
        background-color: #3662E3;
        color: #F9FAFB;
        font-size: 1rem;
        padding: 0.7rem 1.4rem;
        border-radius: 12px;
        border: 1px solid #CDD5E0;
        display: flex;
        align-items: center;
        gap: 10px;
        cursor: pointer;
    }

    .translate-button span{
        margin-top: 2px;
    }

    .number-text{
        text-align: right;
        color: #868b94;
        font-size: 12px;
        font-weight: 400;
        margin-bottom: 15px;
    }

    .textarea{
        width: 100%;
        resize: none;
        background-color: transparent;
        border: none;
        color: #F9FAFB;
        font-size: 1rem;
        font-weight: 600;
        margin-top: 30px;
    }

    .textarea:focus{
        outline: none;
    }

    .switch{
        width: 30px;
        height: 30px;
    }
   
   .active{
    background-color: #4D5562;
    color: #F9FAFB;
   }

   @media screen and (max-width: 1024px){
    .box-container{
        width: auto;
    }
   }

   @media screen and (max-width: 640px){

    .box-container{
        padding: 20px;
    }

    .language-box{
        padding: 0px;
        padding-bottom: 10px; 
    }

    .language-box > div{
        flex-direction: column;
        gap: 5px;
        align-items: flex-start;
    }

    .language-lists{
        gap: 5px;
    }

    li{
        font-size: 13px;
    }

    .switch{
        padding: 15px;
    }

    .translate-button{
        font-size: 0.9rem;
    }
   }
</style>