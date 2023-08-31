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
      recordedNotes: [],
      startTime: null,
      frameThreshold: 50,
    };
  },
  methods: {
    startRecording() {
      this.recording = true;
      this.recordedNotes = [];
      this.startTime = new Date().getTime();
    },
    stopRecording() {
      this.recording = false;
    },
    playRecording() {
      this.recordedNotes.forEach(frame => {
        const time = frame[0].timestamp;
        setTimeout(() => {
          frame.forEach(note => {
            const player = new Tone.Player(`/src/components/piano/${note.note}.mp3`).toDestination();
            player.autostart = true;
          });
        }, time);
      });
    },
    recordNote(data) {
      if (this.recording) {
        const currentTimestamp = new Date().getTime() - this.startTime;
        const lastFrame = this.recordedNotes[this.recordedNotes.length - 1];
        if (lastFrame && currentTimestamp - lastFrame[0].timestamp < this.frameThreshold) {
          lastFrame.push({ note: data.note, key: data.key, timestamp: currentTimestamp });
        } else {
          this.recordedNotes.push([{ note: data.note, key: data.key, timestamp: currentTimestamp }]);
        }
      }
    }
  }
}
</script>