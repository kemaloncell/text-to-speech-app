<template>
  <div>
    <Header />
    <div class="app-container">
      <select>
        <option v-for="voices in voiceList" :key="voices.voiceURI">{{ `${voices.name} - ${voices.lang}` }}</option>
      </select>
      <br />
      <div class="slidercontainer">
        <input class="slider" type="range" min="0.5" max="3" />
      </div>
      <span></span>
      <br />
      <textarea cols="30" rows="10"></textarea>
      <div class="previewContainer">
        <span> </span>
      </div>
      <br />
      <button class="btn red" type="button">
        <span>Please Read!</span>
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
    });
  },
  data() {
    return {
      tts: window.speechSynthesis,
      voiceList: null,
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
  },
};
</script>
