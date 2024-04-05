<template>
  <div class="user-info" style="margin-bottom: 6rem">
    <h1>🙌🏻 Welcome Zayed</h1>
    <hr style="border: 2px solid white; margin: 3rem 0" />
    <h2>🚀 Please start with giving us a short brief about yourself</h2>
    <div class="user-inputs">
      <div class="user-input">
        <h4>Please list 4 of your interests</h4>
        <input class="input" placeholder="Intrests..." clear />
      </div>

      <div class="user-input">
        <h4>A short brief about your past experiences</h4>
        <input class="input" placeholder="Experiances..." clear />
      </div>

      <div class="user-input">
        <h4>Your education history?</h4>
        <input class="input" placeholder="Education ..." clear />
      </div>
    </div>
  </div>

  <hr style="border: 2px solid white; margin: 3rem 0" />
  <h2>🤖️ Enter your business idea</h2>
  <div class="chat">
    <input
      class="input"
      placeholder="Please enter you business idea...🌽"
      v-model="content"
      clear
    />

    <div class="button-block">
      <button type="button" @click="fetchData" class="btn">
        <strong>{{ btnText }}</strong>
        <div id="container-stars">
          <div id="stars"></div>
        </div>
        <div id="glow">
          <div class="circle"></div>
          <div class="circle"></div>
        </div>
      </button>
    </div>

    <div
      class="card"
      v-if="
        !(!!marketingRes || !!constInfoRes || !!toolsInfoRes || !!GTMInfoRes)
      "
    >
      <pre>✅ The answer will be displayed here.</pre>
    </div>
    <div v-else>
      <AnswerBlock
        style="margin-bottom: 4rem"
        :text="marketingRes ? marketingRes : '⌛️ Loading'"
        title="🚀 Marketing Plan"
      />
      <AnswerBlock
        style="margin-bottom: 4rem"
        :text="constInfoRes ? constInfoRes : '⌛️ Loading'"
        title="💰 Financial Plan"
      />
      <AnswerBlock
        style="margin-bottom: 4rem"
        :text="GTMInfoRes ? GTMInfoRes : '⌛️ Loading'"
        title="🪄 Go to Market Strategy"
      />
      <AnswerBlock
        style="margin-bottom: 4rem"
        :text="toolsInfoRes ? toolsInfoRes : '⌛️ Loading'"
        title="🤖
      Here are some helpful tools to get started"
      />
    </div>

    <div
      v-if="!!marketingRes && !!constInfoRes && !!toolsInfoRes && !!GTMInfoRes"
    >
      <hr style="border: 2px solid white; margin: 3rem 0" />

      <h3>
        By using AI technology with online data, we have found the following:
      </h3>

      <div class="precentage">
        <div>
          <h2><b>78% Match</b></h2>
        </div>

        <div class="card">
          <pre>
          ✅ Based on your personal data and your past learning experiances, we belive that your idea: {{
              content
            }} is <b>78%</b> sutiable for you.</pre
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import AnswerBlock from "./components/AnswerBlock.vue";

//Axios instance
const http = axios.create({
  baseURL: "https://api.openai.com/v1/chat",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${import.meta.env.VITE_OPEN_API_KEY}`,
    "OpenAI-Organization": import.meta.env.VITE_ORG_ID,
  },
});

const content = ref("");
const BTN_TEXT = "Submit 🚀";
const btnText = ref(BTN_TEXT);
const btnStatusCounter = ref(0);

const marketingRes = ref("");
const constInfoRes = ref("");
const toolsInfoRes = ref("");
const GTMInfoRes = ref("");

//watch
watch(btnStatusCounter, (newval) => {
  if (newval === 4) {
    btnText.value = BTN_TEXT;
  }
});

//functions
function getMarketingInfo() {
  btnText.value = "Thinking...🤔";
  http
    .post("/completions", {
      model: "gpt-3.5-turbo", //specify the model
      messages: [
        //list down your prompts here
        {
          role: "user", //"system" (to set GPT personality Tone), "user" (Input), or "assistant" (store previous assistant responses))
          content: `List 5 marketing ideas for a my business idea:[${content.value}]`, // ActualPrompt Here
        },
      ],
      temperature: 0.7, //randomness, lower the value the more accurate the result will be
    })
    .then(function (response) {
      console.log(response);
      marketingRes.value = response.data.choices[0].message.content;
    })
    .catch(function (error) {
      console.log(error);
    })
    .finally(() => {
      btnStatusCounter.value += 1;
    });
}

function getCostInfo() {
  btnText.value = "Thinking...🤔";
  http
    .post("/completions", {
      model: "gpt-3.5-turbo",
      messages: [
        {
          role: "user",
          content: `List a full financial plan for this business idea: [${content.value}]`,
        },
      ],
      temperature: 0.7,
    })
    .then(function (response) {
      console.log(response);
      constInfoRes.value = response.data.choices[0].message.content;
    })
    .catch(function (error) {
      console.log(error);
    })
    .finally(() => {
      btnStatusCounter.value += 1;
    });
}

function getToolsInfo() {
  btnText.value = "Thinking...🤔";
  http
    .post("/completions", {
      model: "gpt-3.5-turbo",
      messages: [
        {
          role: "user",
          content: `List 10 tools to help me with (website making, online calls, task management, presentation making) with links for my startup: [${content.value}]`,
        },
      ],
      temperature: 0.7,
    })
    .then(function (response) {
      console.log(response);
      toolsInfoRes.value = response.data.choices[0].message.content;
    })
    .catch(function (error) {
      console.log(error);
    })
    .finally(() => {
      btnStatusCounter.value += 1;
    });
}

function getGTMInfo() {
  btnText.value = "Thinking...🤔";
  http
    .post("/completions", {
      model: "gpt-3.5-turbo",
      messages: [
        {
          role: "user",
          content: `Give me a go to market startegy for my startup idea: [${content.value}] in very clear and precise way.`,
        },
      ],
      temperature: 0.7,
    })
    .then(function (response) {
      console.log(response);
      GTMInfoRes.value = response.data.choices[0].message.content;
    })
    .catch(function (error) {
      console.log(error);
    })
    .finally(() => {
      btnStatusCounter.value += 1;
    });
}

function fetchData() {
  btnText.value = "Thinking...🤔";

  //Parallel requests order
  //Get marketing info
  getMarketingInfo();

  //Get cost-plan info
  getCostInfo();

  //Get tools
  getToolsInfo();

  //GTML Info
  getGTMInfo();
}

//run the request
// fetchData();

//Ask GPT-API
// const askAi = () => {
//   btnText.value = "Thinking...🤔";
//   http
//     .post("/completions", {
//       model: "gpt-3.5-turbo",
//       messages: [{ role: "user", content: content.value }],
//       temperature: 0.7,
//     })
//     .then(function (response) {
//       console.log(response);
//       res.value = response.data.choices[0].message.content;
//     })
//     .catch(function (error) {
//       console.log(error);
//     })
//     .finally(() => {
//       btnText.value = BTN_TEXT;
//     });
// };
</script>

<style>
h1 {
  margin-bottom: 64px;
}
/* 
.chat {
} */
.input {
  width: calc(100% - 20px);
  height: 32px;
  padding: 12px;
  border: none;
  border-radius: 16px;
  box-shadow: 2px 2px 7px 0 rgb(0, 0, 0, 0.2);
  outline: none;
  font-size: 16px;
}

.input:invalid {
  animation: justshake 0.3s forwards;
  color: red;
}

@keyframes justshake {
  25% {
    transform: translateX(5px);
  }
  50% {
    transform: translateX(-5px);
  }

  75% {
    transform: translateX(5px);
  }

  100% {
    transform: translateX-(5px);
  }
}

button {
  cursor: pointer;
  height: 32px;
  font-size: 16px;
  margin-top: 24px;
  background: royalblue;
  color: white;
  padding: 0.7em 1em;
  padding-left: 0.9em;
  display: flex;
  align-items: center;
  border: none;
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.2s;
}

button span {
  display: block;
  margin-left: 0.3em;
  transition: all 0.3s ease-in-out;
}

button svg {
  display: block;
  transform-origin: center center;
  transition: transform 0.3s ease-in-out;
}
/* 
button:hover .svg-wrapper {
  animation: fly-1 0.6s ease-in-out infinite alternate;
}

button:hover svg {
  transform: translateX(1.2em) rotate(45deg) scale(1.1);
}

button:hover span {
  transform: translateX(5em);
}

button:active {
  transform: scale(0.95);
} */
/* 
@keyframes fly-1 {
  from {
    transform: translateY(0.1em);
  }

  to {
    transform: translateY(-0.1em);
  }
} */

.card {
  background: #07182e;
  position: relative;
  display: flex;
  place-content: center;
  place-items: center;
  overflow: hidden;
  border-radius: 16px;
  margin: 24px 0;
  /* max-height: 420px; */
}

.card {
  margin-top: 32px;
}

.card span,
.card pre {
  z-index: 1;
  color: white;
  font-size: 16px;
}

.card::before {
  content: "";
  position: absolute;
  width: 100%;
  background-image: linear-gradient(
    180deg,
    rgb(0, 183, 255),
    rgb(255, 48, 255)
  );
  height: 130%;
  animation: rotBGimg 3s linear infinite;
  transition: all 0.2s linear;
}

/* @keyframes rotBGimg {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
} */

.card::after {
  content: "";
  position: absolute;
  background: #07182e;
  inset: 5px;
  border-radius: 16px;
}
/* .card:hover:before {
  background-image: linear-gradient(180deg, rgb(81, 255, 0), purple);
  animation: rotBGimg 3.5s linear infinite;
} */
.button-block {
  display: flex;
  align-items: center;
  justify-content: end;
}
.btn {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 8rem;
  max-width: 13rem;
  height: 3rem;
  background-size: 300% 300%;
  backdrop-filter: blur(1rem);
  border-radius: 5rem;
  transition: 0.5s;
  animation: gradient_301 5s ease infinite;
  border: double 4px transparent;
  background-image: linear-gradient(#212121, #212121),
    linear-gradient(
      137.48deg,
      #ffdb3b 10%,
      #fe53bb 45%,
      #8f51ea 67%,
      #0044ff 87%
    );
  background-origin: border-box;
  background-clip: content-box, border-box;
}

#container-stars {
  position: fixed;
  z-index: -1;
  width: 100%;
  height: 100%;
  overflow: hidden;
  transition: 0.5s;
  backdrop-filter: blur(1rem);
  border-radius: 5rem;
}

strong {
  z-index: 2;
  font-size: 16px;
  color: #ffffff;
  text-shadow: 0 0 4px white;
}

#glow {
  position: absolute;
  display: flex;
  width: 12rem;
}

.circle {
  width: 100%;
  height: 30px;
  filter: blur(2rem);
  animation: pulse_3011 4s infinite;
  z-index: -1;
}

.circle:nth-of-type(1) {
  background: rgba(254, 83, 186, 0.636);
}

.circle:nth-of-type(2) {
  background: rgba(142, 81, 234, 0.704);
}

.btn:hover #container-stars {
  z-index: 1;
  background-color: #212121;
}

.btn:hover {
  transform: scale(1.1);
}

.btn:active {
  border: double 4px #fe53bb;
  background-origin: border-box;
  background-clip: content-box, border-box;
  animation: none;
}

.btn:active .circle {
  background: #fe53bb;
}

#stars {
  position: relative;
  background: transparent;
  width: 200rem;
  height: 200rem;
}

#stars::after {
  content: "";
  position: absolute;
  top: -10rem;
  left: -100rem;
  width: 100%;
  height: 100%;
  animation: animStarRotate 90s linear infinite;
}

#stars::after {
  background-image: radial-gradient(#ffffff 1px, transparent 1%);
  background-size: 50px 50px;
}

#stars::before {
  content: "";
  position: absolute;
  top: 0;
  left: -50%;
  width: 170%;
  height: 500%;
  animation: animStar 60s linear infinite;
}

#stars::before {
  background-image: radial-gradient(#ffffff 1px, transparent 1%);
  background-size: 50px 50px;
  opacity: 0.5;
}

@keyframes animStar {
  from {
    transform: translateY(0);
  }

  to {
    transform: translateY(-135rem);
  }
}

@keyframes animStarRotate {
  from {
    transform: rotate(360deg);
  }

  to {
    transform: rotate(0);
  }
}

@keyframes gradient_301 {
  0% {
    background-position: 0% 50%;
  }

  50% {
    background-position: 100% 50%;
  }

  100% {
    background-position: 0% 50%;
  }
}

@keyframes pulse_3011 {
  0% {
    transform: scale(0.75);
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0.7);
  }

  70% {
    transform: scale(1);
    box-shadow: 0 0 0 10px rgba(0, 0, 0, 0);
  }

  100% {
    transform: scale(0.75);
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
  }
}

.user-inputs {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.precentage {
  margin-top: 3rem;
  gap: 2rem;
  display: flex;
  flex-direction: row;
  align-items: center;
}
.precentage h2 {
  font-size: 2.3rem;
  width: 40%;
}
</style>
