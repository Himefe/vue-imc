<script setup>
import { ref, watch } from "vue";

import imc from "../imc.vue";

const { levels, calculoImc } = imc();

const grau = ref({});

const form = {
  peso: ref(),
  altura: ref(),
};

const formataPeso = (event) => {
  let value;
  let regex = new RegExp("([0-9]{1})([0-9]{2})", "g");

  value = event.target.value.replace(regex, "$1.$2");
  form.altura.value = Number(value).toFixed(2);
};
</script>

<template>
  <main>
    <div class="leftSide">
      <div>
        <h1>Calcule o seu IMC.</h1>
        <p>
          IMC é a sigla para Índice de Massa Corpórea, parâmetro adotado pela
          Organização Mundial de Saúde para calcular o peso ideal de cada
          pessoa.
        </p>
      </div>
      <form>
        <div class="inputArea">
          <label for="alturaInput">Altura</label>
          <input
            type="number"
            id="alturaInput"
            placeholder="Digite a sua altura. Ex: 1.5 (em metros)."
            v-model="form.altura.value"
            v-on:blur="formataPeso"
          />
        </div>
        <div class="inputArea">
          <label for="pesoInput">Peso</label>
          <input
            type="number"
            id="alturaInput"
            placeholder="Digite o seu peso. Ex: 75 (em kg)."
            v-model="form.peso.value"
          />
        </div>
      </form>
      <div className="buttonArea">
        <button
          className="buttonCalc"
          @click="
            () => (grau.value = calculoImc(form.altura.value, form.peso.value))
          "
        >
          Calcular
        </button>
      </div>
    </div>
    <div class="rightSide">
      <div v-if="grau.value" class="yourIMC">
        <p>
          Seu IMC atual é de
          <strong>{{ grau?.value?.atualImc.toFixed(2) }}</strong>
        </p>
      </div>
      <div class="cards">
        <div
          :class="[
            'card',
            grau.value && item.title === grau.value.title ? 'marked' : '',
          ]"
          v-for="item in levels"
          :key="item"
          :style="{ backgroundColor: item.color }"
        >
          <div className="icon">
            <p v-if="item.icon === 'up'">👍</p>
            <p v-else>👎</p>
          </div>
          <p>{{ item.title }}</p>
          <p>IMC entre {{ item.imc[0] }} e {{ item.imc[1] }}</p>
        </div>
      </div>
    </div>
  </main>
</template>
