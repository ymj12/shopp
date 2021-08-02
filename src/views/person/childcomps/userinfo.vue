<template>
  <div id="user-info">
    <a href="#" class="clear-fix">
      <slot name="user-icon">
        <svg class="privateImage-svg left">
          <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#avatar-default"></use>
        </svg>
      </slot>
    </a>
  </div>
</template>

<script>
export default {
    name:'Userinfo'
}
</script>

<style scoped>
.clear-fix{
     background-color: var(--color-tint);
}
.privateImage-svg {
    background-color: #fff;
}
</style>