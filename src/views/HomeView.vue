<template>
  <div class="home">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <!-- <rangy></rangy> -->
    <!-- <br><br><br> -->
    <!-- ref: -->
    <!-- <refRangy></refRangy> -->
    <!-- <br><br><br> -->
    <newRangy></newRangy>
    <!-- <rangy3></rangy3> -->
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
// import rangy from '@/components/rangy.vue'
import newRangy from '@/components/new-rangy.vue'
// import refRangy from '@/components/ref-rangy.vue'
// import rangy3 from '@/components/rangy3.vue'

export default {
  name: 'HomeView',
  components: {
    // HelloWorld,
    // rangy,
    newRangy,
    // refRangy,
    // rangy3
  }
}
</script>
