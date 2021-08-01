<template>
    <!-- <div> -->
        <Scroll id="tab-menu" ref="scroll">
            <div class="menu-list">
                 <div class="menu-list-item" :class="{active: index===currentindex}" v-for="(item,index) in categroies" :key="index" @click="itemclick(index)">{{item.title}}
                </div>
            </div>
        </Scroll>
    <!-- </div> -->
</template>

<script>
import Scroll from 'components/common/scroll.vue'

export default {
    name:'Tabmenu',
    components:{
        Scroll
    },
    props:{
        categroies:{
            type:Array,
            default(){
                return []
            }
        }
    },
    data(){
        return{
            currentindex:0
        }
    },
    activated() {
      this.$refs.scroll.refresh()
    },
    methods:{
        itemclick(index){
            this.currentindex =index
            this.$emit('selectitem',index)
        }
    }
}
</script>
<style scoped>
  #tab-menu {
    background-color: #f6f6f6;
    height: 100%;
    overflow: hidden;
    width: 100px;
    box-sizing: border-box;
  }

  .menu-list-item {
    height: 45px;
    line-height: 45px;
    text-align: center;
    font-size: 14px;
  }
.menu-list-item.active{
    font-weight: 700;
    color: red;
    background-color: #fff;
    border-left: 3px solid var(--color-high-text);
  }
</style>