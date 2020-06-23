<template>
  <div class="modal-container" :class="{ 'show' : active }">
    <div class="modal-centent">
     <img
      @click="$emit('close-help-modal')"
        class="close-icon"
        src="../../assets/images/icons/close.png"
        alt=""
        title="Close"
      />
      <p class="help-hint">
      Help content goes here.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["active"],
};
</script>

<style lang="scss">
.modal-container {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  z-index: 5;
  background: rgba(255, 255, 255, 0.8);
display: none;
  &.show{
    display: block;
  }

  .modal-centent {
    margin-top: 80px;
    background: #fff;
    width: 820px;
    left: 10px;
    height: 473px;
    border: 3px solid #f5821f;
    border-radius: 10px;
    padding: 15px;

    position: relative;

    .help-hint{
        font-size: 20px;
    }

    .close-icon {
      position: absolute;
      width: 40px;
      top: 10px;
      right: 10px;
      cursor: pointer;
    }
  }
}
</style>
