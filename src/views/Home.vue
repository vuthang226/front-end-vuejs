<template>
  <div id="home">
    
    <div class="header1">
      <MaterialHeader/>
    </div>
    <div class="main">
      <div class="panel-left">
        <MaterialMenu/>
      </div>
      <div class="panel-right">
        <MaterialContent/>
      </div>
    </div>
   
  
  </div>
</template>

<script>
import Vue from 'vue';
import MaterialMenu from '@/components/Material/MaterialMenu.vue';
import MaterialHeader from '@/components/Material/MaterialHeader.vue';
import MaterialContent from '@/components/Material/MaterialContent.vue';
import '@/assets/css/button.css';
import '@/assets/css/icon.css';
import '@/assets/css/input.css';
export default Vue.extend({
  name: 'Home',
  components: {
    MaterialMenu,
    MaterialHeader,
    MaterialContent,
  },
});
</script>
<style>
body {
    margin:0px;
    font-size: 13px;
    font-family: Tahoma !important;
    color:black;
}
#home{
  font-family: Tahoma;
  color:black;
}
.header1{
  width: 100%;
  height: 55px;
}
.main{
  display:flex;
}
.panel-left {
    width: 230px;
    height: calc(100vh - 55px);
    background-color: #0072BC;
    color: #ffffff;
}
.panel-right {
    width: calc(100% - 246px);
    height: calc(100vh - 71px);
    position: absolute;
  
  padding: 8px;
  right: 0px;
}
::-webkit-scrollbar {
    width: 5px !important;
    height: 5px !important;
    z-index: 2
}
::-webkit-scrollbar-track {
    width: 6px;
    background-color: #bbb;
}
::-webkit-scrollbar-thumb {
    background-color: #808080;
    border-radius: 8px;
}
</style>
