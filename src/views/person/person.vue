<template>
<div>
    <Navbar class="Navbar"><div slot="center">YMJ购物街</div></Navbar>
    <Userinfo></Userinfo>
</div>
</template>

<script>
import Navbar from 'components/common/navbar.vue'
import Userinfo from './childcomps/userinfo.vue'


export default {
   name:"person",
    components:{
        Navbar,
        Userinfo
    }
}
</script>
<style scoped>
.Navbar{
    background-color: var(--color-tint);
    color: #fff;
    position: relative;
}
</style>