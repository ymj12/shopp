<template>
     <div class="grid-view" ref="gridView">
        <slot></slot>
     </div> 
</template>

<script>
export default {
    name:'Gridview',
    props:{
        cols:{
            type:Number,
            default:2
        },
        hmargin:{
            type:Number,
            default:8
        },
        vmargin:{
            type:Number,
            default:8
        },
        itemspace:{
            type:Number,
            default:8
        },
        linespace:{
            type:Number,
            default:8
        }
    },
    mounted(){
        setTimeout(this.autolayout, 20)
    },
    updated(){
        this.autolayout()
    },
    methods:{
        autolayout(){
            let gridel = this.$refs.gridView;
            let children = gridel.children;
            gridel.style.padding = `${this.vmargin}px ${this.hmargin}px`
            let itemwith = (gridel.clientWidth - 2*this.hmargin-(this.cols-1)*this.itemspace)/this.cols;
            for(let i =0; i<children.length;i++){
                let item =children[i];
                item.style.width =itemwith +'px';
                if((i+1)% this.cols !==0){
                    item.style.marginRight =this.itemspace +'px'
                }
                if(i>= this.cols){
                    item.style.marginTop = this.linespace +'px'
                }
            }
        }
    }
}
</script>
<style scoped>
  .grid-view {
    display: flex;
    flex-wrap: wrap;
  }
</style>