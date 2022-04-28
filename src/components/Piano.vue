<template>
  <div class="flex justify-center items-start h-64 relative">
    <Key
      v-for="key in keymap"
      :key="key.code"
      :key-info="key" />
  </div>
</template>

<script>
import tunings from '../tunings'
import Key from './Key.vue'

export default {
  name: 'Piano',
  components: {
    Key,
  },
  data() {
    return {
      keymap: tunings
    }
  },
}
</script>
