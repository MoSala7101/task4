<template>
  <div id="app">
    <div class="app-container">
      <div class="preloader"></div>
      <div class="container animate__animated animate__fadeInBottomRight">
        <!-- Header modals-->
        <ResourceModal
          :active="resouceModalState"
          @close-resource-modal="resouceModalState = false"
        />
        <HelpModal
          :active="helpModalState"
          @close-help-modal="helpModalState = false"
        />
        <TaskHeader
          question="What’s in the basket? Choose and write."
          @open-resouce-modal="resouceModalState = true"
          @open-help-modal="helpModalState = true"
        />
        <TaskContent
          :choices="choices"
          :answers="answers"
          :answerPlaces="answerPlaces"
          @choice-selected="choiceSelected"
          @check-answer="checkAnswer"
        />
        <TaskFooter @reset-all="resetAll" @show-answers="showAnswers" />
      </div>
    </div>
  </div>
</template>

<script>
/* import needed components */
import TaskHeader from "./components/TaskHeader";
import TaskContent from "./components/TaskContent";
import TaskFooter from "./components/TaskFooter";
import ResourceModal from "./components/modals/ResourceModal";
import HelpModal from "./components/modals/HelpModal";
export default {
  mounted() {
    this.scalePage();
  },

  created() {
    window.addEventListener("resize", this.scalePage);
    window.addEventListener("zoom", this.scalePage);

    window.addEventListener("load", function() {
      document.querySelector(".preloader").classList.add("hidden");
    });
  },

  components: {
    TaskHeader,
    TaskFooter,
    TaskContent,
    ResourceModal,
    HelpModal,
  },
  data() {
    return {
      choices: [
        { id: 1, text: "scissors", isAvailable: true },
        { id: 2, text: "eraser", isAvailable: true },
        { id: 3, text: "ruler", isAvailable: true },
        { id: 4, text: "bag", isAvailable: true },
        { id: 5, text: "pencil case", isAvailable: true },
        { id: 6, text: "pencil", isAvailable: true },
        { id: 7, text: "book", isAvailable: true },
        { id: 8, text: "pen", isAvailable: true },
      ],
      answers: ["eraser", "ruler", "pencil", "book", "pen"],
      answerPlaces: [
        { id: 1, text: "", isCorrect: false, isFilled: false },
        { id: 2, text: "", isCorrect: false, isFilled: false },
        { id: 3, text: "", isCorrect: false, isFilled: false },
        { id: 4, text: "", isCorrect: false, isFilled: false },
        { id: 5, text: "", isCorrect: false, isFilled: false },
        { id: 6, text: "", isCorrect: false, isFilled: false },
      ],
      currentSelected: "",
      resouceModalState: false,
      helpModalState: false,
    };
  },
  methods: {
    scalePage() {
      /* get the window width */
      let windowWidth = window.innerWidth;

      /* scale 1  */
      let appWidth = 840;

      /* get the app  */
      const theApp = document.querySelector("#app");
      /* set the initial width */
      theApp.style.width = `${appWidth}px`;

      /* check if window has resized */
      if (windowWidth <= 840) {
        /* get the difference between the app width and the window width */
        let widthDif = window.innerWidth - theApp.offsetWidth;
        /* get the percentage of change */
        let diffPercent = widthDif / 840;
        /* calculate the new scale to match the window new width */
        let scale = 1 + diffPercent;
        /* scale the app */
        theApp.style.transform = `scale(${scale})`;
      } else {
        /* if window width > 840 then there is no need to scale the app */
        theApp.style.transform = "scale(1)";
      }
    },

    /**************************************
    TASK CONTENT COMPONENT FUNCTIONS START
    ************************************ */

    /* this function is called when a choice button is clicked */
    choiceSelected(id) {
      /* remove all selected classes */
      let allAnswers = document.querySelectorAll(".answer-btn");
      for (let i = 0; i < allAnswers.length; i++) {
        allAnswers[i].classList.remove("selected");
      }
      /* get the clicked button */
      let clickedBtn = document.getElementById(id);
      /* change clicked button style */
      clickedBtn.classList.add("selected");
      /* update current selected word */
      this.currentSelected = clickedBtn.innerText;
    },

    /* this function is called when an answr place is clicked */
    checkAnswer(id) {
      /* make sure that the is not equal to the dummy place '2' */
      if (id !== 2) {
        let selectedAnswer = this.currentSelected;
        console.log("Value of: checkAnswer -> selectedAnswer", selectedAnswer);
        /* check if the answer is in the answers array */
        let answerIndex = this.answers.indexOf(selectedAnswer);
        if (answerIndex == -1) {
          /* the selected answer is not correct */

          /* get the clicked answer place */
          let clickedPlace = this.answerPlaces.filter(
            (place) => place.id === id
          );

          /* check if there is a choosen choice */
          if (selectedAnswer.length > 0) {
            /* show the select INCORRECT word for 0.5 second */
            clickedPlace[0].text = selectedAnswer;
            clickedPlace[0].isFilled = true;
            setTimeout(() => {
              clickedPlace[0].text = "";
              clickedPlace[0].isFilled = false;
            }, 500);
          }
        } else {
          /* then the clicked place is not the dummy one */
          /* get the clicked place */
          let clickedPlace = this.answerPlaces.filter(
            (place) => place.id === id
          );
          /* check if clicked place is empty */
          if (!clickedPlace.isFilled) {
            clickedPlace[0].isCorrect = true;
            clickedPlace[0].text = selectedAnswer;
            clickedPlace[0].isFilled = true;

            /* remove the CORRECT selected answer button from the available choices */
            let previousChoice = this.choices.filter(
              (choice) => choice.text == selectedAnswer
            );
            previousChoice[0].isAvailable = false;

            /* clear the current selected */
            this.currentSelected = "";

            /* get all vailable places (without the dummy one) */
            let allPlaces = this.answerPlaces.filter((place) => place.id !== 2);

            /* get the filled places */
            let filledPlaces = allPlaces.filter(
              (place) => place.isFilled === true
            );

            /* check the count of filled places */
            if (filledPlaces.length == 5) {
              /* then all answer fields are filled with correct answers */
              /* disable all remaining buttons */
              let allAnswersButtons = document.querySelectorAll(".answer-btn");
              for (let i = 0; i < allAnswersButtons.length; i++) {
                allAnswersButtons[i].classList.add("disabled");
              }
            }
          }
        }
      }
    },

    /************************************
    TASK CONTENT COMPONENT FUNCTIONS END
    ********************************** */

    /**************************************
    TASK FOOTER COMPONENT FUNCTIONS START
    ************************************ */

    /* this function is called when the reset icon is pressed */
    resetAll() {
      /* reset answer buttons */
      let allAnswersButtons = document.querySelectorAll(".answer-btn");
      for (let i = 0; i < allAnswersButtons.length; i++) {
        allAnswersButtons[i].classList.remove("disabled");
        allAnswersButtons[i].classList.remove("notVisible");
      }

      /* reset choices availability */
      this.choices.map((choice) => (choice.isAvailable = true));

      /* reset answerPlaces availability and text */
      this.answerPlaces.filter((place) => {
        place.text = "";
        place.isFilled = false;
        place.isCorrect = false;
      });
    },

    /* this function is called when the show answers icon is pressed */
    showAnswers() {
      let availablePlaces = this.answerPlaces.filter((place) => place.id !== 2);
      console.log("Value of: showAnswers -> availablePlaces", availablePlaces);

      /* loop for the places and puth the CORRECT answers in each place */
      for (let i = 0; i < this.answers.length; i++) {
        /* put each answer in an answerplace */
        availablePlaces[i].isFilled = true;
        availablePlaces[i].text = this.answers[i];
        availablePlaces[i].isCorrect = true;
      }

      /* hide all CORRECT choices buttons */
      let selectedAnswersButtons = document.querySelectorAll(".answer-btn");
      for (let i = 0; i < selectedAnswersButtons.length; i++) {
        /* get the button text */
        let buttonText = selectedAnswersButtons[i].innerText;
        /* check if this button text is thhe answers array */
        let answerIndex = this.answers.indexOf(buttonText);
        if (answerIndex != -1) {
          /* add disabled and notVisible classes to the button */
          selectedAnswersButtons[i].classList.add("notVisible");
        }
        selectedAnswersButtons[i].classList.add("disabled");
      }
    },

    /**************************************
    TASK FOOTER COMPONENT FUNCTIONS END
    ************************************ */
  },
};
</script>

<style lang="scss">
/* resetting */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  overflow: hidden;
}

/* import fonts */
@font-face {
  font-family: "Sassoon";
  src: url("./assets/fonts/SassoonSansStd.otf");
}
/* global classes */
.notVisible {
  opacity: 0 !important;
}
.hidden {
  display: none;
}

#app {
  display: flex;
  flex-flow: column;
  justify-content: center;
  height: 630px;
  margin: 0 auto;
  font-family: Sassoon;
  transform-origin: top left;
  font-style: normal;
  font-weight: 200;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 0 !important;
  transition: all 500ms ease-in-out !important;

  .preloader {
    background-image: url("./assets/images/icons/preloader.gif");
    background-position: center;
    background-repeat: no-repeat;
    z-index: 3000;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 200px;
    height: 200px;
  }
  .container {
    width: 100%;
    height: 100%;
  }
}
</style>
