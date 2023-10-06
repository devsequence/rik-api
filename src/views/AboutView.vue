<template>
  <div class="about">
    <h1>{{textAbout}}</h1>
  </div>
</template>
<script>

  export default {
    name: 'AboutView',
    data(){
      return{
        textAbout : 'This is an about page'
      }
    },
    components: {

    }
  }
</script>