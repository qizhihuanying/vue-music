<template>
  <div id="keyboard">
    <button v-for="(note, index) in notes" :key="note" @click="playNote(note, index)">
      {{ keys[index] }}
    </button>
  </div>
</template>

<script>
import * as Tone from 'tone'

export default {
  data() {
    return {
      notes: ['C3', 'D3', 'E3', 'F3', 'G3', 'A3', 'B3', 'C4', 'D4', 'E4', 'F4', 'G4', 'A4', 'B4', 'C5', 'D5', 'E5', 'F5', 'G5', 'A5', 'B5', 'C6'], // 22 notes
      keys: ['z', 'x', 'c', 'v', 'b', 'n', 'm', 'a', 's', 'd', 'f', 'g', 'h', 'j', 'q', 'w', 'e', 'r', 't', 'y', 'u', 'i'] // 22 keys
    };
  },
  methods: {
    playNote(note, index) {
      const player = new Tone.Player(`/src/components/piano/${note}.mp3`).toDestination();
      player.autostart = true;
      this.$emit('notePlayed', {note: note, key: this.keys[index]});
    }
  },
  mounted() {
    window.addEventListener('keydown', e => {
      const key = e.key;
      const index = this.keys.indexOf(key);
      if (index !== -1) {
        this.playNote(this.notes[index], index);
      }
    });
  },
  beforeDestroy() {
    window.removeEventListener('keydown');
  }
}
</script>