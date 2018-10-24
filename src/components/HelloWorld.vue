<template>
  <div class="hello">
    <h1>{{ hint }}</h1>
    <button @click="startRecording">record</button>
    <button @click="stopRecording">stop</button>
    <audio :src="sounds" controls></audio>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      answerIsShow: false,
      answerChoose: 0,
      mediaRecorder: null,
      chunks: [],
      record: {
        APPKEY: 'X0Ya6UgzMY1eq5wQQr9qVZGM',
        APPSECRET: 'FAdxiK4XWMwwalBM5oO5HnMDujAZlH0N',
        FILE: '16k_test.pcm',
        FORMAT: 'wav',
        RATE: '16000',
        DEV_PID: '1537',
        token_url: 'https://cors-anywhere.herokuapp.com/https://speech.googleapis.com/v1/operations/recognize?key=AIzaSyCSKeObICewV8DrxcYMcnLnlseSSt33Zr8',
        access_token: '',
      },
      sounds: '',
      hint: '略過',
    };
  },
  mounted() {
    const token_url = 'https://cors-anywhere.herokuapp.com/https://speech.googleapis.com/v1/operations/recognize?key=AIzaSyCSKeObICewV8DrxcYMcnLnlseSSt33Zr8';
    const vm = this;

    try {
      // webkit shim
      window.AudioContext = window.AudioContext || window.webkitAudioContext;
      navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia;
      window.URL = window.URL || window.webkitURL;

      // this.audio_context = new AudioContext();
    } catch (e) {
      alert('No web audio support in this browser!');
    }

    navigator.getUserMedia({ audio: true }, this.startUserMedia, (e) => {});
    // fetch(token_url,{method:"GET",mode:"cors",credentials: "same-origin",
    //   headers:{
    //   }}).then(res => res.text())
    //   .then(function(response) {vm.getToken(response); })
    //   .catch(error => console.error('Error:', error));
  },
  methods: {
    startUserMedia(stream) {
      const config = {
        sampleRate: 44100,
        numChannels: 1,
      };
      this.mediaRecorder = new MediaRecorder(stream);
      if (this.mediaRecorder.notSupported) {
        this.hint = '瀏覽器不支援錄音中請略過';
      }
      // var input = this.audio_context.createMediaStreamSource(stream);
      // this.recorder = new Recorder(input, config);
    },
    startRecording(button) {
      const vm = this;
      const chunks = this.chunks;
      this.mediaRecorder.start();
      this.mediaRecorder.ondataavailable = function (e, chuncks) {
        debugger;
        chunks.push(e.data);
      };

      // this.recorder && this.recorder.record();
    },
    stopRecording(button) {
      const vm = this;

      this.mediaRecorder.stop();

      this.mediaRecorder.onstop = function (e) {
        console.log('recorder stopped');

        const blob = new Blob(vm.chunks, { type: 'audio/x-wav' });

        const audioURL = window.URL.createObjectURL(blob);
        vm.chunks = [];
        vm.sounds = audioURL;
      };
      this.hint = '錄音結束';
      // this.recorder && this.recorder.stop();
      // create WAV download link using audio data blob
      // this.createDownloadLink();
      // this.recorder.clear();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
