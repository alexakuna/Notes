<template>
<div class="modal">
<slot></slot>
</div>
</template>

<script>
export default {
  name: 'ModalComponent',
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  z-index: 40;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
