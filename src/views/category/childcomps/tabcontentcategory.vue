<template>
<div > 
<Gridview :cols="3" :linespace="15" :vmargin="20" v-if="subcategories.list">
        <div class="item" v-for="(item,index) in subcategories.list" :key="index">
        <a :href="item.link">
            <img :src="item.image" alt="" @load="categoryimages">
            <div class="item-text">{{item.title}}</div>
        </a>
    </div>
</Gridview>
</div>
</template>
<script>
import Gridview from 'components/common/gridview/Gridview'


export default {
    name:'TabcontentCategory',
    components:{
        Gridview
    },
    props:{
        subcategories:{
            type:Object,
            default(){
                return {}
            }
        }
    },
    methods:{
        categoryimages(){
            this.$emit('categoryimgload')
         }
    }
}
</script>
<style scoped>
.item{
  text-align: center;
  font-size: 12px;
}
img{
    width: 80%;
}
.item-text{
    margin-top: 15px;
}
</style>