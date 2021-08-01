<template>
  <div id="category" >
      <Navbar class="navcenter"><div slot="center">商品分类</div></Navbar>
    <div class="content">
      <Tabmenu :categroies="categroies" @selectitem="selectitem"></Tabmenu>
      <div id="tab-item">
      <TabcontentCategory :subcategories="showSubcategory"></TabcontentCategory>
      <Tabcontrol :titles="['综合', '新品', '销量']" @tabClick="tabClick"></Tabcontrol>
      <Tabcontentdetail :categorydetail="showCategoryDetail"></Tabcontentdetail>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
      <div>12</div>
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


  export default {
    name: "Category",
    components:{
      Navbar,
      Tabmenu,
      TabcontentCategory,
      Tabcontrol,
      Tabcontentdetail
    },
    data() {
      return {
        categroies:[],
        categoryData:{},
        currentindex:-1,
        currentType:'pop'
      }
    },
    created(){
     this.getCategroy()
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
      // this.$refs.tabcontrol1.currentindex=index;
      // this.$refs.tabcontrol2.currentindex=index;
    },
    }

  }
</script>

<style scoped>
  #category {
    height: 100vh;
  }
.navcenter{
  background-color:var(--color-tint);
  color: #fff;
  /* position: relative; */
}
  .content {
    position: absolute;
    left: 0;
    right: 0;
    top: 44px;
    bottom: 49px;
    display: flex;
  }
  #tab-item{
    height: 100%;
    flex: 1;
  }
</style>
