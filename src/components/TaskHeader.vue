<template>
  <div class="task-header">
    <p class="question">
      {{ question }}
    </p>
    <div class="buttons">
      <img class="icon-btn" @click="$emit('open-resouce-modal')" src="../assets/images/icons/resource.png" alt="" />
      <img class="icon-btn" @click="$emit('open-help-modal')" src="../assets/images/icons/help.png" alt="" />
    </div>
  </div>
</template>

<script>
export default {
  props: ["question"],
};
</script>

<style lang="scss" scoped>
.task-header {
  height: 80px;
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin: 0 auto;

  .question {
    margin: 0 20px;
    font-size: 26px;
    font-weight: 100;
  }

  .buttons {
    .icon-btn {
      width: 45px;
      height: 40px;
      margin: 0 15px;
      cursor: pointer;
    }
  }
}
</style>
