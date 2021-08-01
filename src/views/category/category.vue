<template>
  <div id="category" >
      <Navbar class="navcenter"><div slot="center">商品分类</div></Navbar>
    <div class="content">
      <Tabmenu :categroies="categroies" @selectitem="selectitem"></Tabmenu>
      <div id="tab-item">
          <Tabcontrol :titles="['综合', '新品', '销量']" class="tabcontrol" ref="tabcontrol1" @tabClick="tabClick" v-show="istabfixed"></Tabcontrol>
        <Scroll class="contents" ref="scroll" @scroll="contentscroll">
          <TabcontentCategory :subcategories="showSubcategory" @categoryimgload="categoryimgload"></TabcontentCategory>
          <Tabcontrol :titles="['综合', '新品', '销量']" ref="tabcontrol2" @tabClick="tabClick"></Tabcontrol>
          <Tabcontentdetail :categorydetail="showCategoryDetail"></Tabcontentdetail>
        </Scroll>
       <Backtop @click.native="backClick" v-show="isShowBackTop"/>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from 'components/common/navbar.vue'  
import {getCategroy,getSubcategory,getCategoryDetail} from '../../network/categroy'
import Tabmenu from './childcomps/tabmenu.vue'
import TabcontentCategory from './childcomps/tabcontentcategory.vue'
import Tabcontrol from 'components/content/tabcontrol.vue'
import Tabcontentdetail from './childcomps/tabcontentdetail.vue'
import Scroll from 'components/common/scroll.vue'
import {debounce} from 'common/untils'
import Backtop from 'components/common/backtop.vue'



  export default {
    name: "Category",
    components:{
      Navbar,
      Tabmenu,
      TabcontentCategory,
      Tabcontrol,
      Tabcontentdetail,
      Scroll,
      Backtop

    },
    data() {
      return {
        categroies:[],
        categoryData:{},
        currentindex:-1,
        currentType:'pop',
        istabfixed:false,
        taboffsettop:0,
        isShowBackTop:false,
      }
    },
    created(){
     this.getCategroy()
    },
    mounted(){
    const refresh =debounce(this.$refs.scroll.refresh,100)
        this.$bus.$on('itemimgloads', ( ) => {
          refresh()
    })
  },
      activated() {
      // console.log('djl');
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      this.$refs.scroll.refresh()
    },
    deactivated() {
      // console.log('jjj');
      this.saveY = this.$refs.scroll.getScrollY()
    },
    computed:{
      showSubcategory(){
        if(this.currentindex ===-1) return{}
        return this.categoryData[this.currentindex].subcategories
      },
      showCategoryDetail(){
        if(this.currentindex ===-1) return []
        return this.categoryData[this.currentindex].categorydetail[this.currentType]
      }
    },
    // 组件创建完后调用
    methods:{
     getCategroy(){
       getCategroy().then(res=>{
         this.categroies =res.data.category.list
          for (let i = 0; i < this.categroies.length; i++) {
            this.categoryData[i] = {
              subcategories: {},
              categorydetail: {
                'pop': [],
                'new': [],
                'sell': []
              }
            }
          }
         this.getSubcategories(0)
       })
     },
     getSubcategories(index){
       this.currentindex = index;
       const mailkey  =this.categroies[index].maitKey;
       getSubcategory(mailkey).then(res => {
         this.categoryData[index].subcategories =res.data
         this.categoryData = {...this.categoryData}
         this.getCategoryDetails('pop')
         this.getCategoryDetails('new')
         this.getCategoryDetails('sell')
       })
     },
     getCategoryDetails(type){
       const miniWallkey = this.categroies[this.currentindex].miniWallkey;
        getCategoryDetail(miniWallkey,type).then(res =>{
          this.categoryData[this.currentindex].categorydetail[type] = res
          this.categoryData = {...this.categoryData}
        })
     },
     selectitem(index){
       this.getSubcategories(index)
     },
     categoryimgload(){
       this.taboffsettop =this.$refs.tabcontrol2.$el.offsetTop
     },
     contentscroll(position){
       this.isShowBackTop= (-position.y)>1000
       this.istabfixed=position.y< -this.taboffsettop
     },
      backClick(){
      this.$refs.scroll.scrollTo(0,0)
    },
      tabClick(index){
      switch(index){
        case 0:
          this.currentType='pop'
          break  
          case 1:
          this.currentType='new'
          break
          case 2:
          this.currentType='sell'
          break
      }
      this.$refs.tabcontrol1.currentindex=index;
      this.$refs.tabcontrol2.currentindex=index;
    },
    }

  }
</script>

<style scoped>
  #category {
    height: 100vh;
    position: relative;
  }
.navcenter{
  background-color:var(--color-tint);
  color: #fff;
  /* position: relative; */
}
.tabcontrol{
    position: relative;
    z-index: 9;
}
  .content {
    position: absolute;
    left: 0;
    right: 0;
    top: 44px;
    bottom: 49px;
    display: flex;
  }
  .contents{
    overflow: hidden;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 100px;
    right: 0;

  }
  #tab-item{
    height: 100%;
    flex: 1;
  }
</style>
