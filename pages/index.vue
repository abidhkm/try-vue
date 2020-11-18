<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <template v-if="options.length > 0">
        <v-card-title class="question">{{ question }}</v-card-title>
        <radio v-bind:options="options" v-model="answer" v-on:answer-select="updateAnswer"></radio>
        <div class="flex-center">
          <v-btn v-if="!submitted" @click="clickHandler">Submit</v-btn>
          <v-btn v-if="submitted" @click="proceedHandler">Proceed</v-btn>
          <p
            class="status"
            v-if="submitted"
          >{{isAnsCorrect ? "Correct answer" : `Wrong answer!! \n Correct answer is option ${correctAns}`}}</p>
        </div>
      </template>
    </v-col>
  </v-row>
</template>
<script>
const sampleData = [
  {
    question: 'How many states are there in India?',
    options: [28, 29, 30, 32],
  },
  {
    question: 'How many rivers are there in Kerala?',
    options: [44, 46, 56, 53],
  },
]

import Radio from '~/components/Radio'

export default {
  components: {
    Radio,
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData: async function (params) {
      const ref = this.$fire.firestore.collection('questions')
      try {
        const snapshot = await ref.get()
        let items = [];
        snapshot.forEach((doc, index) => {
          items.push({
              ...doc.data()
            });
        })
        this.documents = items;
        this.options = this.documents[0].options;
        this.question = this.documents[0].text
        this.correctAns = this.documents[0].answer
      } catch (e) {
        return Promise.reject(e)
      }
    },
    proceedHandler: function (params) {
      console.log("proceed")
      this.qNo++;
      this.options = this.documents[this.qNo].options;
        this.question = this.documents[this.qNo].text
        this.correctAns = this.documents[this.qNo].answer
        this.submitted = false
    },
    clickHandler: function (params) {
      
    
      if(this.answer === this.options[this.correctAns]) {
        console.log("correct")
        this.isAnsCorrect = true;
      } else {
        console.log("wrong")
        this.isAnsCorrect = false;
      }

      this.submitted = true;
      
    },
    updateAnswer: function (params) {
      
      this.answer = params
    },
  },
  data() {
    return {
      qNo:0,
      documents: [],
      question: "",
      options: [],
      answer: null,
      correctAns: null,
      submitted: false,
      isAnsCorrect: false
    }
  },
}
</script>

<style  scoped>
.question {
  justify-content: center;
}
.flex-center {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.status {
  margin: 15px 0;
}
</style>