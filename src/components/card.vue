<template>
  <div class="card">
    <div class="image_container">
        <img :src="image" class="image" />
    </div>
    <div>{{title}}</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  props:{
      image : {
          required : true
      },
      title : {
          required : true
      }
  },
  async mounted(){
  }
}
</script>

<style lang="scss">
    .card{
        margin : 10px;
        width : 200px;
        height : 200px;
        .image_container{
            width : 99%;
            height : 90%;
            border : 1px solid black;
            .image {
                width : 100%;
                height : 100%;
            }
        }
    }
</style>
