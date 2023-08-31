<template>
  <div id="recorder">
    <button @click="startRecording">Start Recording</button>
    <button @click="stopRecording">Stop Recording</button>
    <button @click="playRecording">Play Recording</button>
    <Keyboard @notePlayed="recordNote"/>
  </div>
</template>

<script>
import Keyboard from './Keyboard.vue'
import * as Tone from 'tone'

export default {
  components: {
    Keyboard
  },
  data() {
    return {
      recording: false,
      recordedNotes: []
    };
  },
  methods: {
    startRecording() {
      this.recording = true;
      this.recordedNotes = [];
    },
    stopRecording() {
      this.recording = false;
    },
    playRecording() {
      const synth = new Tone.Synth().toDestination();
      this.recordedNotes.forEach(note => {
        synth.triggerAttackRelease(note, "8n");
      });
    },
    recordNote(note) {
      if (this.recording) {
        this.recordedNotes.push(note);
      }
    }
  }
}
</script>