<template>
  <div class="modal-container" :class="{ show: active }">
    <div class="modal-centent">
      <img
        @click="$emit('close-resource-modal')"
        class="close-icon"
        src="../../assets/images/icons/close.png"
        alt=""
        title="Close"
      />
      <div class="image-container">
        <div class="resource-image"></div>
      </div>
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
  &.show {
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
    padding: 10px;

    position: relative;

    .image-container {
      width: 100%;
      height: 100%;
      overflow: auto;

      .resource-image {
        width: 850px;
        height: 630px;
        background-image: url("../../assets/images/dummy.jpg");
        background-size: cover;
        background-position: center;
      }
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
