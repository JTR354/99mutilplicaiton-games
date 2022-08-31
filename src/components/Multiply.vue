<template>
  <div class="multiply">
    <div class="card">
      <h1>{{ state.qa[0] }}</h1>
      <h1>×</h1>
      <h1>{{ state.qa[1] }}</h1>
      <h1>=</h1>
      <input
        class="answer"
        @focus="handleFocus"
        :class="{ error: state.isError }"
        type="tel"
        :value="answer"
        @input="handleChange"
        :autocomplete="false"
        maxlength="2"
        @blur="submit"
        autofocus
        @keyup.enter="submit"
      />
      <!-- <span class="score">分数: {{ score }}</span> -->
    </div>
    <Transition>
      <section v-if="score >= 10" class="victory" @click="restart">
        <img src="/v.jpg" />
      </section>
    </Transition>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from "vue";
let cache = new Set();
onUnmounted(() => {
  cache.clear();
  cache = null;
});
function createNumber() {
  return Math.max((Math.random() * 100) % 10 >> 0, 2);
}
function createQa(level = 0) {
  const result = [createNumber(), createNumber()];
  if (
    (level < 10 && cache.has(result.toString())) ||
    cache.has([...result].reverse().toString())
  ) {
    return createQa(level + 1);
  }
  cache.add(result.toString());
  return result;
}
const answer = ref("");
const score = ref(0);
const state = reactive({ qa: createQa(), isError: false });
function setTitle(score = 0) {
  document.title = `得分: ${score}`;
}
onMounted(() => {
  setTitle();
});
let lock = false;
const submit = (e) => {
  setTimeout(() => {
    lock = false;
  }, 100);
  if (lock) {
    return;
  }
  lock = true;
  const value = answer.value;
  if (value) {
    const [one, two] = state.qa;
    const result = confirm(`${one}×${two}=${value}？`);
    if (result) {
      const isTrue = one * two === +value;
      if (isTrue) {
        state.qa = createQa();
        answer.value = "";
        score.value++;
        setTitle(score.value);
      }
      state.isError = !isTrue;
    }
  }
};
const handleChange = (e) => {
  answer.value = e.target.value;
  state.isError = false;
};
const handleFocus = (e) => {
  e.target.select();
};

const restart = () => {
  confirm("再来一局?") && location.reload();
};
</script>

<style scoped>
.victory {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-items: center;
}

.victory img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.error {
  outline: 2px solid red !important;
  color: red !important;
  font-weight: bold;
}

.answer {
  /* width: 3.2em; */
  /* height: 3.2em; */
  font-size: 3.2em;
  line-height: 1;
  text-align: center;
  display: inline;
  width: 1.5em;
  border: none;
  outline: 1px solid skyblue;
  /* color: #333; */
}

.card {
  width: 50%;
  height: 50%;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  position: relative;
  padding: 3rem;
  /* color: #333; */
}

.card input {
  background: transparent;
}

.score {
  position: fixed;
  bottom: 0.5em;
  color: red;
  font-size: 2em;
  font-weight: bold;
}

.multiply {
  position: fixed;
  top: 0;
  left: 0;
  opacity: 1;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background-color: #213547;
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease-in-out;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

@media (prefers-color-scheme: light) {
  .multiply {
    color: #213547;
    background-color: #ffffff;
  }
  a:hover {
    color: #747bff;
  }
  button {
    background-color: #f9f9f9;
  }
}
</style>
