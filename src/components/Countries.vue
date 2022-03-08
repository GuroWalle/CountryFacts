<template>
  <div class="countries">
    <form class="countries__form" v-on:submit.prevent="fetchNewCountry">
      <input
        class="countries__input"
        type="text"
        placeholder="Search for a country..."
        v-model="countrySearch"
      />
    </form>
    <div class="countries__data">
      <div class="data__facts">
        <h1 class="facts__title">{{ commonName }}</h1>
        <h2>{{ officialName }}</h2>
        <h2>{{ alternativeSpelling }}</h2>
        <h2>{{ capital }}</h2>
        <h2>{{ subregion }}</h2>
      </div>
      <div class="data__images">
        <img :src="flag" alt="" />
        <img :src="coatOfArms" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      countrySearch: "",

      commonName: "",
      officialName: "",
      alternativeSpelling: "",
      capital: "",
      subregion: "",
      flag: "",
      coatOfArms: "",
    };
  },

  created() {
    this.fetchNewCountry();
  },

  methods: {
    async fetchNewCountry() {
      console.log(this.countrySearch);
      const url = `https://restcountries.com/v3.1/name/${this.countrySearch}`;
      const result = await fetch(url);

      try {
        const output = await result.json();
        this.commonName = output[0].name.common;
        this.officialName = output[0].name.official;
        this.alternativeSpelling = output[0].altSpellings[2];
        this.capital = output[0].capital[0];
        this.subregion = output[0].subregion;
        this.flag = output[0].flags.svg;
        this.coatOfArms = output[0].coatOfArms.svg;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
.countries__form {
  background: pink;
  display: flex;
  justify-content: center;
  padding: 2rem;
}

.countries__input {
  border: 2px solid black;
  border-radius: 2rem;
  padding: 1rem;
  width: 25rem;
  font-size: var(--font--caption);
}

.countries__data {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin: var(--size--medium);
  background: burlywood;
  padding: 5rem;
}

.data__facts h1 {
  margin: var(--size--small);
  font-size: var(--font--heading);
}

.data__facts h2 {
  margin: var(--size--small);
  font-size: var(--font--body);
}

.data__images {
  display: flex;
  flex-direction: column;
}

.data__images img {
  max-width: 20rem;
  max-height: 25rem;
  margin: var(--size--big) 0;
}
</style>