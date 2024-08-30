<template>
  <div class="q-pa-md row justify-center chat-div" ref="chatDiv">
    <!--div style="width: 100%; max-width: 800px"-->
    <div style="width: 80%; margin: auto;"  >
      <div v-if="chatMessages.length !== 0">
        <div class="text-h6 text-bold">
          Chat Exemplo!
        </div>   
        <div style="margin-bottom: 40px;"></div>

      </div>   
      <!--q-chat-message
        label="Hoje, 15:07"
      /-->
      <TransitionGroup name="list" tag="div">
        <div v-for="message in chatMessages" :key="message.id">
          <q-chat-message v-if="message.author === 'me'"
            :text="[message.text]"
            sent
            text-color="white"
            bg-color="primary"
            :name="message.author"
            avatar="https://cdn.quasar.dev/img/avatar4.jpg"
            size="6"
            style="text-align: right"
          >
          </q-chat-message>
          <q-chat-message v-if="message.author === 'bot'"
            bg-color="amber"
            text-html
            :text="[message.text]"
            :name="message.author"
            avatar="https://cdn.quasar.dev/img/avatar2.jpg"
            size="6"
            style="text-align: left"
          >
          </q-chat-message>
        </div>
        <q-chat-message v-if="botThinking"
            bg-color="amber"
            avatar="https://cdn.quasar.dev/img/avatar2.jpg"
            name="bot"
            size="6"
            style="text-align: left"
        >
          <q-spinner-dots size="2rem" v-if="botThinking" />
        </q-chat-message>
      </TransitionGroup>
    </div>
    <div v-if="chatMessages.length === 0">
      <div class="text-h4">Converse comigo!</div>
      <div class="text-h6 q-my-md text-bold">
        O que você gostaria de saber?
      </div>
    </div>   
    <div class="input-container">
      <q-input 
        label="Comece a digitar..." 
        class="text-grey-8" 
        outlined v-model="question" 
        @keydown.enter="sendQuestion()" 
      >
        <template v-slot="append">
          <q-btn 
            color="secondary" 
            label="Enviar" 
            class="q-ma-sm"  
            @click="sendQuestion()" 
          />
        </template>
      </q-input>
    </div> 
  </div>
</template>

<script setup>
import { ref } from 'vue';

const chatMessages = ref([])
const botThinking = ref(false)
const question = ref('')
const chatDiv = ref(null)

const sendQuestion = () => {
  console.log('chatMessages:', chatMessages);
  chatMessages.value.push ({
    text: question.value,
    author: 'me'
  })
  question.value = ''
  botThinking.value = true;
  setTimeout(() => {
    answerQuestion();
    botThinking.value = false;
  }, 500);
}

const answerQuestion = () => {
  chatMessages.value.push ({
    text: 'Curitiba é um município brasileiro, capital do estado do Paraná, localizado a 934 metros de altitude no Primeiro Planalto Paranaense,[9] a mais de 110 quilômetros do Oceano Atlântico,[13] distante 1 386 km a sul de Brasília, capital federal. Com 1 773 718 habitantes,[14] é o município mais populoso do Paraná e da região Sul, além de ser o 8.º do país, segundo Censo Demográfico realizado pelo IBGE para 2022.',
    author: 'bot'
  })
  let computedStyle = getComputedStyle(chatDiv.value)
  let height = computedStyle.getPropertyValue("height")
  window.scrollTo(0, parseFloat(height.substring(0, height.length - 2)));
}

</script>