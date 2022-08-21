<template>
  <div>
    <div class="container">
      <input v-model="cardNumber" placeholder="Номер карты" maxlength="16" />
      <button @click="hiddenCard">Скрыть номер</button>
    </div>
    <div class="container">
      <input v-model="cash" placeholder="Количество" />
      <input v-model="procent" placeholder="Процент" />
      <button @click="cashProcent">Посчитать</button>
      <div v-if="res">Результат: {{ res }}</div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({})
export default class HomeView extends Vue {
  cardNumber = "";

  res: number | null = null;

  procent = "0.5";

  cash = "1000";

  hiddenCard() {
    if (this.cardNumber.length < 16) return;
    let regexp = /((?<=\d{4})\d(?=\d{4}))/g;
    this.cardNumber = this.cardNumber.replace(regexp, "*");
  }

  cashProcent() {
    const ratio = () => {
      if (this.procent.indexOf(".") == -1) return 1;
      return 10 ** (this.procent.length - this.procent.indexOf(".") - 1);
    };

    const cash = BigInt(this.cash);
    const procent = BigInt(Number(this.procent) * ratio());
    this.res = Number((cash * procent) / (100n * BigInt(ratio())));
  }
}
</script>

<style lang="stylus">
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
input, button {
  width: 20%
  height: 40px;
  margin: 8px;
}
.container {
  margin: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
