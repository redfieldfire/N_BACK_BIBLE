<script setup>
import { ref } from 'vue'

const count = ref(2)
const currentBook = ref("")
const pastBooks = ref([])
const answerPastBook = ref("")
const timeToAnswer = ref(false)

const answer1 = ref("")
const answer2 = ref("")

const color1 = ref("")
const color2 = ref("")

const bibleBooks = [
  //OLD TESTAMENT
  "Génesis",
  "Éxodo",
  "Levítico",
  "Números",
  "Deuteronomio",
  "Josué",
  "Jueces",
  "Rut",
  "1 Samuel",
  "2 Samuel",
  "1 Reyes",
  "2 Reyes",
  "1 Crónicas",
  "2 Crónicas",
  "Esdras",
  "Nehemías",
  "Ester",
  "Job",
  "Salmos",
  "Proverbios",
  "Eclesiastés",
  "Cantares",
  "Isaías",
  "Jeremías",
  "Lamentaciones",
  "Ezequiel",
  "Daniel",
  "Oseas",
  "Joel",
  "Amós",
  "Abdías",
  "Jonás",
  "Miqueas",
  "Nahum",
  "Habacuc",
  "Sofonías",
  "Hageo",
  "Zacarías",
  "Malaquías",
  
  //NEW TESTAMENT
  "Mateo",
  "Marcos",
  "Lucas",
  "Juan",
  "Hechos",
  "Romanos",
  "1 Corintios",
  "2 Corintios",
  "Gálatas",
  "Efesios",
  "Filipenses",
  "Colosenses",
  "1 Tesalonicenses",
  "2 Tesalonicenses",
  "1 Timoteo",
  "2 Timoteo",
  "Tito",
  "Filemón",
  "Hebreos",
  "Santiago",
  "1 Pedro",
  "2 Pedro",
  "1 Juan",
  "2 Juan",
  "3 Juan",
  "Judas",
  "Apocalipsis"
];

const getRandomBook = () => {
    currentBook.value = randomBook()
}

const randomBook = (except = -1) => {
    var randomNumber = Math.floor(Math.random() * bibleBooks.length)
    while (except == randomNumber) {
        randomNumber = Math.floor(Math.random() * bibleBooks.length)
    }
    return bibleBooks[randomNumber]
}

const addRandomBook = () => {
    pastBooks.value.push(currentBook.value)
}

const addAnswerBook = () => {
    answerPastBook.value = pastBooks.value.shift()
}

const setTimeToAnswer = (value) => {
    timeToAnswer.value = value
}

const nextAction = () => {
    getRandomBook()
    addRandomBook()
    if (pastBooks.value.length > count.value) {
        setTimeToAnswer(true)
        addAnswerBook()
        setAnswers()
    }
}

const setAnswers = () => {
    if (Math.random() * 10 > 5) {
        answer1.value = answerPastBook.value
        answer2.value = randomBook(pastBooks.value.indexOf(answerPastBook.value))
    } else {
        answer1.value = randomBook(pastBooks.value.indexOf(answerPastBook.value))
        answer2.value = answerPastBook.value
    }
}

const guessAction = (value, button) => {
    if(answerPastBook.value == value){
        getRandomBook()
        addRandomBook()
        addAnswerBook()
        setAnswers()

        color1.value = ""
        color2.value = ""
    }
    else {
       if (button == 1) {
        color1.value = "red"
        color2.value = "green"
       }
       else {
        color2.value = "red"
        color1.value = "green"
       }
    }
}

</script>

<template>
    <div class="center column gap-30">
        <label class="actual-book-title">{{currentBook}}</label>
        <button class="button-next" v-if="!timeToAnswer" @click="nextAction">Siguiente!</button>
        <div v-else class="center row gap-30">
            <button class="button-answer" :style="{backgroundColor: color1}" @click="guessAction(answer1, 1)">{{answer1}}</button>
            <button class="button-answer" :style="{backgroundColor: color2}" @click="guessAction(answer2, 2)">{{answer2}}</button>
        </div>
        <label class="answer-list-item" v-for="(book, index) in pastBooks" :key="index">{{( index + 1 ) + " -> " + book}}</label>
        <h2 class="answer">Answer: {{answerPastBook}}</h2>
    </div>
</template>

<style scoped>
    .center {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .column {
        flex-direction: column;
    }
    
    .row {
        flex-direction: row;
    }

    .gap-30 {
        gap: 30px;
    }

    .actual-book-title {
        font-size: 10rem;
    }

    .button-answer {
        font-size: 3rem;
    }

    .button-next {
        font-size: 3rem;
    }

    .answer-list-item {
        font-size: 2rem;
    }

    .answer {
        font-size: 3rem;
    }
</style>
