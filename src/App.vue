<template>
  <div>
    <navBarSec/>
    <router-view/>
  </div>
</template>
<script>
// @ is an alias to /src
import navBarSec from '@/components/navBarSec.vue'

export default {
  name: 'Home',
  components: {
    navBarSec
  }
}
</script>
<style>

</style>
