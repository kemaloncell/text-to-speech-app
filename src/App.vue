<template>
  <div>
    <Header />
    <div class="app-container">
      <select v-model="selectedVoice">
        <option v-for="voices in voiceList" :key="voices.voiceURI">{{ voices.name }}</option>
      </select>
      <br />
      <div class="slidercontainer">
        <input class="slider" type="range" min="0.5" max="3" />
      </div>
      <span></span>
      <br />
      <textarea cols="30" rows="10" v-model="textToSpeech"></textarea>
      <div class="previewContainer">
        <span></span>
      </div>
      <br />
      <button class="btn red" type="button" @click="speak">
        <span>Please read the text!</span>
      </button>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
export default {
  name: 'App',
  components: {
    Header,
  },
  created() {
    this.getVoices().then((voices) => {
      this.voiceList = voices;
      this.selectedVoice = 'Microsoft Tolga - Turkish (Turkey)';
    });
  },
  data() {
    return {
      tts: window.speechSynthesis,
      voiceList: null,
      selectedVoice: null,
      textToSpeech: 'I will read the following text according to the chosen language',
    };
  },
  methods: {
    getVoices() {
      let intervalID;
      return new Promise((resolve) => {
        intervalID = setInterval(() => {
          if (this.tts.getVoices().length > 0) {
            resolve(this.tts.getVoices());
            clearInterval(intervalID);
          }
        }, 10);
      });
    },
    speak() {
      let toSpeak = new SpeechSynthesisUtterance(this.textToSpeech);
      toSpeak.voice = this.voiceList.find((v) => v.name == this.selectedVoice) || null;
      this.tts.speak(toSpeak);
      console.log(toSpeak.voice);
    },
  },
};
</script>
