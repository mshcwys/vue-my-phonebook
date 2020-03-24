<template>
  <div id="app">
    <index />
  </div>
</template>

<script>
import index from "./components/index";

export default {
  name: 'app',
  components:{
    index
  },
  data(){
    return{

    }
  
  },
  methods:{

  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
#app{
  margin: 0 auto;
  width: 50%;
}
/*媒体查询 */
@media screen and (max-width: 1280px){
  #app{
    width: 90%;
  } 
}

</style>
