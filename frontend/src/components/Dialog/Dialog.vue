<template>
<div id="dialog-background" @click.stop="$emit('hide-dialog')" @keydown.stop>
  <div id="dialog" @click.stop>
    <slot></slot>
  </div>
</div>
</template>

<style>
#dialog-background{
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.6);
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 6;
}
#dialog{
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  padding: 20px 40px;
}
</style>

<script>
export default {
  name: 'Dialog',
  emits: ['hide-dialog']
}
</script>
