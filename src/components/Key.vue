<template>
  <div
    class="rounded-b relative border"
    :class="{
      'bg-white w-16 h-full border border-gray-300 shadow z-10': !isBlackKey,
      'bg-gray-900 border-gray-900 w-12 h-32 -mx-6 z-20': isBlackKey,
    }">
    <div v-show="isPlaying" class="absolute inset-0 rounded-b opacity-50 bg-green-200 border-2 border-green-500"></div>
    <div class="absolute top-0 -mt-8 inset-x-0 text-center flex items-center justify-center">
      <div
        class="font-semibold"
        :class="{
          'text-gray-600': !isPlaying,
          'text-green-500': isPlaying
        }">
        {{ keyInfo.label }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Key',
  props: {
    keyInfo: {
      type: Object,
      required: true
    },
  },
  computed: {
    isBlackKey() {
      return this.keyInfo.note.includes('#')
    }
  },
  data() {
    return {
      isPlaying: false,
      audioContext: null,
      oscillatorNode: null,
      gainNode: null,
    }
  },
  created() {
    window.addEventListener('keydown', this.handleKeydown)
    window.addEventListener('keyup', this.handleKeyup)
  },
  destroyed() {
    window.removeEventListener('keydown', this.handleKeydown)
    window.removeEventListener('keyup', this.handleKeyup)
  },
  methods: {
    handleKeydown(e) {
      if (e.code === this.keyInfo.code) {
        this.play()
      }
    },
    handleKeyup(e) {
      if (e.code === this.keyInfo.code) {
        this.stop()
      }
    },
    play() {
      if (!this.isPlaying) {
        this.isPlaying = true

        const AudioContext = window.AudioContext || window.webkitAudioContext
        this.audioContext = new AudioContext()

        // create OscillatorNode
        this.oscillatorNode = this.audioContext.createOscillator()
        this.oscillatorNode.type = 'sine'
        this.oscillatorNode.frequency.value = this.keyInfo.frequency
        this.oscillatorNode.connect(this.audioContext.destination)

        // create GainNode
        this.gainNode = this.audioContext.createGain()
        this.gainNode.gain.value = 1

        this.oscillatorNode.start(0) // then tell it to play
      }
    },
    stop() {
      this.gainNode.gain.value = 0 // turn volume down so we don't get a weird pop
      this.oscillatorNode.stop(.1) // then tell note to stop playing. delay it to help avoid the popping sound
      this.isPlaying = false

      this.gainNode = null
      this.oscillatorNode = null
      this.audioContext = null
    }
  },
}
</script>