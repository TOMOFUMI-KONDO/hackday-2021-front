<template>
  <h1>ガチャ最適化サービス</h1>
  <form @submit="submit">
    <section id="gachas">
      <h2>引きたいガチャ</h2>
      <ul>
        <li v-for="(gacha, index) in gachas" :key="gacha.id">
          <input
            type="checkbox"
            :id="gacha.id"
            :value="gacha.value"
            name="gachas"
            v-model="checkedGachas"
          />
          <label :for="gacha.id">{{ gacha.value }}</label>
          <label
            v-if="checkedGachas.includes(gacha.value)"
            :for="`${gacha.id}-least-times`"
          >
            <input
              type="number"
              :id="`${gacha.id}-least-times`"
              name="leastTimes"
              v-model="leastTimes[index]"
            />
            回
          </label>
        </li>
      </ul>
    </section>
    <section id="max-cost">
      <h2>限度額</h2>
      <input
        type="number"
        id="max-cost-input"
        name="maxCost"
        v-model="maxCost"
      />
      <label for="max-cost-input">円</label>
    </section>
    <section id="modes">
      <h2>モード</h2>
      <ul>
        <li v-for="mode in modes" :key="mode.id">
          <input
            type="radio"
            :id="mode.id"
            :value="mode.value"
            name="mode"
            v-model="checkedMode"
          />
          <label :for="mode.id">{{ mode.value }}</label>
        </li>
      </ul>
    </section>
    <input type="submit" value="送信" />
  </form>
  <p>result: {{ result }}</p>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data: function() {
    return {
      gachas: [
        { id: "pazdora", value: "パズドラ" },
        { id: "umamusume", value: "ウマ娘" },
        { id: "monst", value: "モンスト" }
      ],
      checkedGachas: [],
      leastTimes: [],
      maxCost: null,
      modes: [
        { id: "mode-a", value: "モードA" },
        { id: "mode-b", value: "モードB" },
        { id: "mode-c", value: "モードC" }
      ],
      checkedMode: [],
      result: ""
    };
  },
  methods: {
    async submit(event) {
      event.preventDefault();

      const response = await axios.post(process.env.VUE_APP_API_ENDPOINT, {
        gachas: this.checkedGachas,
        leastTimes: this.leastTimes,
        maxCost: this.maxCost,
        mode: this.checkedMode
      });
      const responseJson = JSON.parse(response.data.body);
      console.log(responseJson);

      this.result = Object.keys(responseJson).map(
        key => `${key}: ${responseJson[key]}`
      );
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
