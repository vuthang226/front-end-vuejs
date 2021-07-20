<template>
  <div class="custom-select" :tabindex="tabindex" @blur="open = false">
    <div class="selected isFocus" :class="{ open: open,isHideBorder:isHideBorder,isRequird:isRequird }" @click="open = !open">
      {{ selected }}
      <div class="icon-drop" :class="{isMove:isHideAdd}"></div>
      <div class="icon-select-add" @click="$emit('add')" :class="{isHideAdd:isHideAdd}"></div>
    </div>
    <div class="items" :class="{ selectHide: !open }">
      <div
        v-for="(option, i) of arrayOptions"
        :key="i"
        :class="{iss:i == index}"
        @click="
          select(option,i)
        "
      
      >
        {{ option }}
      </div>
    </div>
    
  </div>
</template>

<script>
export default {
  name:"Select", 
  props: {
    options: {
      type: Array,
      default: null,
    },
    default: {
      type: String,
      required: false,
      default: null,
    },
    tabindex: {
      type: Number,
      required: false,
      default: 0,
    },
    isHideAdd:{
      type:Boolean,
      default:false,
    },
    isHideBorder:{
      type:Boolean,
      default:false,
    },
    isRequird:{
      type:Boolean,
      default:false,
    },
  },
  data() {
    return {
        
      selected:this.options.length > 0
        ? this.options[0].value
        : null,
      open: false,
      index:-1,
      optionsData:this.options,

    };
  },
  methods:{
        select(option,k){
                this.selected = option;
                this.open = false;
                for (let i=0; i<this.optionsData.length; i++ ){
                    if(Object.values(this.optionsData[i])[1] == this.selected){
                        this.$attrs.value = Object.values(this.optionsData[i])[0];
                        this.$emit('input', this.$attrs.value);
                    }
                }  
                this.$emit('change', option);
                this.index = k;
        },
  },
  computed:{
    updatedefault:function(){
        return this.$attrs.value  
    },
    
    arrayOptions:function(){
        let arr = new Array;
        for (let i=0; i<this.optionsData.length; i++ ){
            arr[i] = Object.values(this.optionsData[i])[1];
        }
        return arr;

    }

  },
  watch:{
    updatedefault:function(){
        for (let i=0; i<this.optionsData.length; i++ ){
            if(this.$attrs.value == Object.values(this.optionsData[i])[0]){
                this.selected = Object.values(this.optionsData[i])[1];
            }
        }  
    },
    open:function(){
      if(this.options != null ){
        for(let i = 0;i < this.optionsData.length;i++){
          if(Object.values(this.optionsData[i])[1] == this.selected){
            this.index = i;
          }
        }
      }
    },
    options:function(){
        this.optionsData=this.options
    }
    
  },
  
}
</script>

<style scoped>
.iss{
  background-color:#d7e9f4 !important;
}
.isHideAdd{
  display: none;
}
.custom-select .isRequird{
  border: 1px solid red;
}
.isMove{
  right: 1em !important;
}
.custom-select .isHideBorder{
  border: none;
}
.custom-select {
  position: relative;
  width: 100%;
  outline: none;
  height: 30px;
}

.custom-select .selected {
    display: flex;
    align-items: center;
    background-color:white;
    height: 28px;
    font-size: 13px;
    color: black;
    outline: none;    
    cursor: pointer;
    padding-left: 16px;

}
.selected {
    border: 1px solid #bbbbbb;  
}


.custom-select .selected.open {
  border: 1px solid #0072bc;
}

.custom-select .selected .icon-drop {
  position: absolute;
  content: "";
  top: 14px;
  right: 2.5em;
  width: 0;
  height: 0;
  border: 5px solid transparent;
  border-color: black transparent transparent transparent;

}

.custom-select .items {
  color: black;
  overflow: hidden;
  position: absolute;
  background-color: #fff;
  max-height: 120px;
  overflow: auto;
  left: 0;
  right: 0;
  top:35px;
  z-index: 20;
  box-shadow: 1px 1px 1px 1px #E9EBEE;
}

.custom-select .items div {
  color: black;
  cursor: pointer;
  padding-left: 36px;
  user-select: none;
  height: 30px;
  display: flex;
    align-items: center;
    background-color:white;
    border: none;
    outline: none;
    cursor: pointer;
    padding-left: 25px;
}

.custom-select .items div:hover {
  background-color:rgb(193, 221, 241);
}

.selectHide {
  display: none;
}

.icon-select-add{
  position: absolute;
  top: 10px;
  right: 0.5em;
  width: 0;
  height: 0;
  background: url('../assets/icon/IconSprite.png') no-repeat 0 -48px;
	width: 15px;
	height: 15px;
}


</style>