<script setup lang="ts">
import { reactive } from "vue";
import clipboardIcon from "../assets/clipboard.svg";
import clipboardCheckIcon from "../assets/clipboard-check.svg";

const state = reactive({
  length: 12,
  password: "",
  useUppercase: true,
  useLowercase: true,
  useNumbers: true,
  useSymbols: true,
  copied: false,
});

const generatePassword = () => {
  state.copied = false;

  const uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  const lowercase = "abcdefghijklmnopqrstuvwxyz";
  const numbers = "0123456789";
  const symbols = "!@#$%^&*()_+[]{}<>?";

  let candidate = "";
  if (state.useUppercase) candidate += uppercase;
  if (state.useLowercase) candidate += lowercase;
  if (state.useNumbers) candidate += numbers;
  if (state.useSymbols) candidate += symbols;

  if (!candidate) {
    state.password = "Select at least one character set";
    return;
  }

  let result = "";
  for (let i = 0; i < state.length; i++) {
    const randIndex = Math.floor(Math.random() * candidate.length);
    result += candidate[randIndex];
  }

  state.password = result;
};

const copyToClipboard = () => {
  state.copied = true;
  navigator.clipboard.writeText(state.password);
};
</script>

<template>
  <div class="generator">
    <h1 class="title">Password Generator</h1>

    <div
      class="output"
      :class="{ clickable: state.password }"
      @click="copyToClipboard"
    >
      <input
        readonly
        class="viewbox"
        type="text"
        :value="state.password"
        placeholder="Click Generate"
      />

      <img
        v-if="state.password"
        :src="state.copied ? clipboardCheckIcon : clipboardIcon"
        :alt="state.copied ? 'Copied' : 'Copy'"
        class="copy-icon"
      />
    </div>

    <div class="settings">
      <div class="slider-wrapper" data-min="8" data-max="32">
        <p class="length-title">Length: {{ state.length }}</p>
        <input
          type="range"
          class="slider"
          min="8"
          max="32"
          v-model="state.length"
        />
      </div>

      <div class="setting">
        <input type="checkbox" id="uppercase" v-model="state.useUppercase" />
        <label for="uppercase">Include Uppercase</label>
      </div>

      <div class="setting">
        <input type="checkbox" id="lowercase" v-model="state.useLowercase" />
        <label for="lowercase">Include Lowercase</label>
      </div>

      <div class="setting">
        <input type="checkbox" id="number" v-model="state.useNumbers" />
        <label for="number">Include Numbers</label>
      </div>

      <div class="setting">
        <input type="checkbox" id="symbol" v-model="state.useSymbols" />
        <label for="symbol">Include Symbols</label>
      </div>
    </div>

    <button class="generate-button" @click="generatePassword">
      Generate Password
    </button>
  </div>
</template>

<style scoped>
.generator {
  width: 400px;
  padding: 1rem 1.5rem;
  background-color: var(--dark);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow);
}

.title {
  margin-bottom: 2rem;
  font-size: 1.75rem;
  text-align: center;
  color: var(--white);
}

.output {
  position: relative;
  width: 100%;
  height: 5rem;
  margin-bottom: 3.5rem;
  overflow: clip;
  cursor: initial;

  &.clickable {
    cursor: pointer;
  }

  & .viewbox {
    width: 100%;
    height: 100%;
    border-radius: var(--radius-sm);
    border: none;
    outline: none;
    font-size: 1.25rem;
    font-weight: 800;
    text-align: center;
    line-height: 1;
    color: var(--white);
    background-color: var(--bg);
    caret-color: transparent;
    pointer-events: none;

    &::placeholder {
      text-transform: uppercase;
      color: var(--white);
    }
  }

  & .copy-icon {
    position: absolute;
    top: 0.25rem;
    right: 0.25rem;
    width: 1.5rem;
    height: 1.5rem;
    filter: brightness(0) invert(1);
    opacity: 0.5;
    transition: all var(--transition);

    &:hover {
      opacity: 1;
    }
  }
}

.slider-wrapper {
  position: relative;
  display: flex;
  gap: 0.75rem;
  width: 100%;
  height: var(--setting-height);
  border-radius: var(--radius-sm);
  padding-inline: 0.5rem;
  align-items: center;
  color: var(--white);
  background-color: var(--bg);

  &::before,
  &::after {
    position: relative;
    font-size: 1.125rem;
    font-weight: 700;
  }

  &::before {
    content: attr(data-min);
  }

  &::after {
    content: attr(data-max);
  }
}

.length-title {
  position: absolute;
  bottom: 105%;
  left: 0.5rem;
  font-size: 1.125rem;
  font-weight: 800;
  text-transform: uppercase;
  color: var(--muted);
  pointer-events: none;
}

.slider {
  -webkit-appearance: none;
  appearance: none;
  flex: 1;
  height: 0.5rem;
  border-radius: var(--radius-sm);
  outline: none;
  padding: 0;
  margin: 0;
  background-color: var(--accent);
  cursor: pointer;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  background-color: var(--white);
  cursor: pointer;
  transition: all var(--transition);
}

.slider::-webkit-slider-thumb:hover {
  background-color: var(--gray);
  transform: scale(1.2);
}

.slider::-moz-range-thumb {
  width: 1.5rem;
  height: 1.5rem;
  border: 0;
  border-radius: 50%;
  background-color: var(--white);
  cursor: pointer;
  transition: all var(--transition);
}

.slider::-moz-range-thumb:hover {
  background-color: var(--gray);
}

.settings {
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.setting {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: var(--setting-height);
  padding-inline: 1.5rem;
  border-radius: var(--radius-sm);
  color: var(--white);
  background-color: var(--bg);
}

.setting input {
  position: absolute;
  opacity: 0;
}

.setting input + label {
  font-size: 1.125rem;
}

.setting input + label::before,
.setting input + label::after {
  content: "";
  position: absolute;
  transition: var(--transition-cubic);
  transform: translateY(-50%);
  top: 50%;
  right: var(--cb-gap);
  cursor: pointer;
}

.setting input + label::before {
  height: 2rem;
  width: 3.25rem;
  border-radius: var(--radius-lg);
  background-color: var(--muted);
}

.setting input + label::after {
  height: var(--cb-size);
  width: var(--cb-size);
  border-radius: 50%;
  right: calc(var(--cb-gap) + var(--cb-size));
  background-color: var(--white);
}

.setting input:checked + label::before {
  background-color: var(--accent);
  transition: all var(--transition-cubic);
}

.setting input:checked + label::after {
  right: calc(var(--cb-gap) + 0.25rem);
}

.setting input:focus + label::before {
  box-shadow: var(--shadow-3);
}

.setting input:disabled + label::before,
.setting input:disabled + label::after {
  cursor: not-allowed;
}

.setting input:disabled + label::before {
  background-color: var(--disabled);
}

.setting input:disabled + label::after {
  background-color: var(--disabled-2);
}

.generate-button {
  width: 100%;
  height: 3rem;
  border-radius: var(--radius-sm);
  border: none;
  outline: none;
  letter-spacing: 0.075rem;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--white);
  background-image: linear-gradient(
    135deg,
    var(--accent) 0%,
    var(--accent-2) 100%
  );
  cursor: pointer;
  transition: all var(--transition-fast);
}

.generate-button:active {
  transform: translateY(-5%);
  box-shadow: var(--shadow-2);
}
</style>
