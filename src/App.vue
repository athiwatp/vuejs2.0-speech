<template>
  <div id="app" :style="{ background: styleColor }">
    <h1> {{ msg }}  </h1>
  </div>
</template>

<script>
/* global webkitSpeechRecognition, webkitSpeechRecognitionEvent, webkitSpeechGrammarList */
var SpeechRecognition = SpeechRecognition || webkitSpeechRecognition
var SpeechGrammarList = SpeechGrammarList || webkitSpeechGrammarList
var SpeechRecognitionEvent = SpeechRecognitionEvent || webkitSpeechRecognitionEvent
console.log(SpeechRecognition, SpeechGrammarList, SpeechRecognitionEvent)
var recognition = new SpeechRecognition()
var speechRecognitionList = new SpeechGrammarList()
// set variable //
// speechRecognitionList.addFromString(grammar, 1)
recognition.grammars = speechRecognitionList
recognition.continuous = false
recognition.lang = 'en-US'
recognition.interimResults = true
recognition.maxAlternatives = 1
console.log(recognition, speechRecognitionList)
var color = ['red', 'black']
export default {
  name: 'app',
  components: {},
  mounted () {
    var vm = this
    recognition.start()
    recognition.onresult = function (event) {
      vm.msg = event.results[0][0].transcript
      vm.voiceColor(vm.msg)
    }
    recognition.onspeechend = function () {
      recognition.stop()
    }
    speechRecognitionList.addFromString(this.grammar, 1)
    console.log(vm.grammar)
  },
  data () {
    return {
      msg: 'Hello',
      grammar: '#JSGF V1.0; grammar colors; public <color> = ' + color.join(' | ') + ' ;',
      styleColor: ''
    }
  },
  methods: {
    voiceColor (color) {
      console.log('Text voice Revcive : ', color)
      if (color === 'red' || color === 'Red') this.styleColor = 'red'
      else if (color === 'black' || color === 'Black') this.styleColor = 'black'
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /*color: #2c3e50;*/
  margin-top: 60px;
}
</style>
