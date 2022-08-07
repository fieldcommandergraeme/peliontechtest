<template>
  <div class="home">
    <RandomActivity />
  </div>
</template>

<script>
// @ is an alias to /src
import RandomActivity from '@/components/RandomActivity.vue'

export default {
  name: 'HomeView',
  components: {
    RandomActivity
  }
}
</script>
