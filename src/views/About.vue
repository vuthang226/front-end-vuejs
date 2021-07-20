<template>
  <div class="about">
    <h1>This is an about page</h1>
    <Selectt
    :options="options"
    v-model="a"
    :isHideAdd="true"
    
    />
  </div>
</template>
<script>
import Selectt from "@/components/Select.vue";

export default {
  name: 'About',
  data(){
    return{
      a:2,
      options:[{id:1,value:'Ngày'},{id:2,value:'Tháng'},{id:3,value:'Năm'}]
    }
  },
  components:{
    Selectt
  }
  
}
</script>
