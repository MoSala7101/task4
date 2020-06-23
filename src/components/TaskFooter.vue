<template>
  <div class="task-footer">
    <div class="buttons">
      <img class="reset-btn" @click="$emit('reset-all')" src="../assets/images/icons/reset.png" alt="" />
      <img class="solve-btn" @click="$emit('show-answers')" src="../assets/images/icons/solve.png" alt="" />
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
.task-footer {
  height: 85px;
  background: #DEECF6;
  .buttons {
    display: grid;
    grid-template-columns: 60% 40%;
    align-items: center;
    justify-content: flex-start;
    height: 100%;
    .reset-btn {
      width: 54px;
      height: 47px;
      margin: 0 20px;
      cursor: pointer;
    }
    .solve-btn {
      width: 66px;
      height: 49px;
      margin: 0 20px;
      cursor: pointer;



    }
  }
}
</style>
