<template>
<Gridview>
    <Categoryitem v-for="(item,index) in categorydetail" :key="index" :goodsitem="item"></Categoryitem>
</Gridview>
</template>

<script>
import Categoryitem from './categoryitem.vue'
import Gridview from 'components/common/gridview/Gridview'

export default {
    name:'Tabcontentdetail',
    components:{
        Categoryitem,
        Gridview
    },
    props:{
        categorydetail:{
            type:Array,
            default(){
                return []
            }
        }
    }
}
</script>
<style scoped>

</style>