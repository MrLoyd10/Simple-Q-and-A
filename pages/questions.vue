<script setup>

  const data = useQuestions();
  const questionIndex = ref(0);
  const currentAnswer = ref(null);

  const storedAnswer = ref([]);

  console.log(data);

  //Current question we are in.
  const currentQuestion = computed(() => {
    return data.questions[questionIndex.value]
  })

  // Randomize the answer option index
  const currentAnswerOptionIndex = computed(() => {
    if (storedAnswer.value[questionIndex.value]) {
      console.log(`There is a data.`);
      return storedAnswer.value[questionIndex.value].indexUsed;
    }

    const options = Array.from({ length: currentQuestion.value.answers.length }, (_, index) => index);

    for (let i = options.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [options[i], options[j]] = [options[j], options[i]];
    }

    return options;
  });

  onUpdated(() => {
    // This code will execute after the component's DOM has been updated
    console.log('v-for loop is complete or buttons have been rendered');
    
    const tempStored = storedAnswer.value[questionIndex.value];

    if (!tempStored) {
      console.log("There is no stored value.");
      return;
    }

    if (tempStored.isCorrect) {
      document.getElementById(`answer-option-${tempStored.answerIndex}`).classList.add("bg-green-500");
    } else {
      document.getElementById(`answer-option-${tempStored.answerIndex}`).classList.add("bg-red-500");
    }
  });
  

  //Check if answer is correct or not
  const checkAnswer = (answer, index) => {
    if (currentAnswer.value != null) {
      console.log("Changing Answer...");
      document.getElementById(`answer-option-${currentAnswer.value.index}`).classList.remove("bg-green-500");
      document.getElementById(`answer-option-${currentAnswer.value.index}`).classList.remove("bg-red-500");
    }

    let isCorrect = null;
    if (answer.correct) {
      console.log("Correct");
      isCorrect = true;
      document.getElementById(`answer-option-${index}`).classList.add("bg-green-500");
    } else {
      console.log("Incorrect");
      isCorrect = false;
      document.getElementById(`answer-option-${index}`).classList.add("bg-red-500");
    }

    currentAnswer.value = {
      text: answer.text,
      isCorrect: isCorrect,
      index: index
    }

    console.log(currentAnswer.value);
  }

  //Go to the previos
  const goToPreviosQuestion = () => {
    if (questionIndex.value <= 0) {
      console.log("No Previous Question")
      return;
    }

    questionIndex.value--;
  };


  //Go to the next question
  const goToNextQuestion = () => {
    
    storedAnswer.value[questionIndex.value] = {
      answer: currentAnswer.value.text,
      isCorrect: currentAnswer.value.isCorrect,
      answerIndex: currentAnswer.value.index,
      indexUsed: currentAnswerOptionIndex.value
    };

    console.log(storedAnswer.value);
    
    if (questionIndex.value < data.questions.length - 1) {
      questionIndex.value++;
    } else {
      console.log("Finish Question")
    }
  };


</script>

<template>
  <div>

    <div class="min-h-screen bg-cyan-900 flex flex-col justify-center align-middle p-6">

      <h3 class="text-center mb-5 text-gray-300"> Question {{ questionIndex + 1 }} out of {{ data.questions.length }}</h3>

      <h1 class="text-2xl font-extrabold text-white text-center mb-10">{{ currentQuestion.question }}</h1>

      <div class="flex justify-center align-middle mb-10 flex-wrap gap-5">
        <ClientOnly>
          <button
            v-for="index in currentAnswerOptionIndex"
            :key="currentQuestion.answers[index].text"
            :id="`answer-option-${index}`"
            class="text-lg px-5 py-2 border border-gray-300 rounded-md bg-blend-hard-light hover:bg-cyan-950 text-white"
            @click="checkAnswer(currentQuestion.answers[index], index)"
          >
            {{ currentQuestion.answers[index].text }}
          </button>
        </ClientOnly>
      </div>

      <div class="flex justify-center space-x-4">
        <button 
          class="border border-gray-400 w-40 py-2 text-center text-gray-400 rounded-sm outline-none hover:bg-gray-400 hover:bg-opacity-10"
          @click="goToPreviosQuestion"
        >
          Previous
        </button>
        <button 
          class="bg-green-500 w-40 py-2 text-center text-black rounded-sm outline-none hover:bg-green-400"
          @click="goToNextQuestion"
        >
          Next
        </button>
      </div>
      
    </div>
  </div>
</template>

<style>
* {
  font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
</style>
