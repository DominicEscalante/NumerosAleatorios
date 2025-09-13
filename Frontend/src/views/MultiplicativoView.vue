<template>
  <div>
    <Navbar />
    <Background />
  </div>
</template>

<script>
import Background from '../components/Background.vue'
import Navbar from '@/components/Navbar.vue'

export default {
  name: 'MultiplicativoView',
  components: {
    Background,
    Navbar,
  },
}
</script>
