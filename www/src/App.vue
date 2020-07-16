<template>
  <div class="pt-12">
    <h1 class="font-bold text-2xl mb-16 text-center">Let's make some noise <span class="ml-1">🎉</span></h1>
    <Piano />
  </div>
</template>

<script>
import Piano from './components/Piano.vue'

export default {
  name: 'App',
  components: {
    Piano
  },
  mounted() {
    const { timeZone, locale } = Intl.DateTimeFormat().resolvedOptions()
    fetch('/api/pageview', {
      method: 'POST',
      body: JSON.stringify({ timeZone, locale }),
      headers: { 'Content-Type': 'application/json' }
    })
  },
}
</script>
