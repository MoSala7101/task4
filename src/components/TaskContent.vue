<template>
  <div class="task-content">
    <div class="question-container">
      <img
        class="question-image"
        src="../assets/images/background.png"
        alt=""
      />

      <div class="choices-container">
        <div class="available-answers">
          <button
            v-for="choice in choices"
            :key="choice.id"
            :class="{ notVisible: !choice.isAvailable }"
            :id="`choice-${choice.id}`"
            class="answer-btn"
            @click="$emit('choice-selected', `choice-${choice.id}`)"
          >
            {{ choice.text }}
          </button>
        </div>
      </div>

      <div class="answers-fields-container">
        <div
          class="answer-container"
          v-for="place in answerPlaces"
          :key="place.id"
          @click="$emit('check-answer', place.id)"
          :class="{ notVisible: place.id == 2 }"
        >
          <p class="answer-text" v-if="place.isFilled == true">
            {{ place.text }}
          </p>
          <img
            v-if="place.isCorrect == true"
            class="correct-sign"
            src="../assets/images/icons/correct.png"
            alt=""
          />
          <img
            v-if="place.isCorrect == false && place.isFilled == true"
            class="incorrect-sign blinking"
            src="../assets/images/icons/incorrect.png"
            alt=""
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["choices", "answers", "answerPlaces"],
  data() {
    return {};
  },
  methods: {},
};
</script>

<style lang="scss">
.task-content {
  width: 840px;
  height: 455px;
  background: #0fa0c5;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px auto 0 auto;
  z-index: 2;

  .question-container {
    background: #fff;
    width: 96%;
    margin: 0 auto;
    height: 473px;
    border: 3px solid #f5821f;
    border-radius: 10px;
    position: absolute;
    left: 2%;
  }

  .question-image {
    width: 100%;
    position: absolute;
    bottom: 0;
    height: 65%;
  }

  .choices-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 170px;
    width: 100%;
  }

  .available-answers {
    width: 600px;
    min-width: 480px;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    .answer-btn {
      width: fit-content;
      margin: 5px;
      cursor: pointer;

      font-size: 28px;
      border: 2px solid #0fa0c5;
      padding: 1px 15px;
      color: #000;

      height: 55px;
      font-weight: 600;
      background: #fff;
      border-radius: 10px;

      &.selected {
        background: #0fa0c5;
        color: #fff;
      }

      &.disabled {
        opacity: 0.6;
      }
    }
  }

  .answers-fields-container {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 130px;
    z-index: 4;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 0 5px;

    .answer-container {
      height: 55px;
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      cursor: pointer;

      .answer-text {
        margin-top: 5px;
        font-size: 32px;
      }

      .correct-sign,
      .incorrect-sign {
        width: 26px;
      }
    }
  }
}

.blinking {
  animation: blinking 500ms ease-in-out;
}

/* Animation */
@keyframes blinking {
  0% {
    opacity: 0;
  }
  25% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  75% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
