<template>
  <div class="countries">
    <form class="countries__form" v-on:submit.prevent="fetchNewCountry">
      <!--.prevent prevnts the site to refresh-->

      <input
        type="text"
        placeholder="Search for a country..."
        v-model="countrySearch"
        autocomplete="off"
        class="countries__input"
      />
    </form>

    <p class="countries__info" v-if="onMainSite">
      Find basic facts about every country in the world!
    </p>
    <p class="countries__error" v-if="countryFound">No country found...</p>

    <div class="countries__data" v-if="visible">
      <div class="data__facts">
        <h1>{{ commonName }}</h1>
        <h2>
          <p>Official name:</p>
          {{ officialName }}
        </h2>
        <h2>
          <p>Native name:</p>
          {{ nativeName }}
        </h2>
        <h2>
          <p>Capital:</p>
          {{ capital }}
        </h2>
        <h2>
          <p>Location:</p>
          {{ continent }}, {{ subregion }}
        </h2>
        <h2>
          <p>Population:</p>
          {{ population }}
        </h2>
      </div>
      <div class="data__images">
        <img :src="flag" alt="" />
        <p>Flag</p>
        <img :src="coatOfArms" alt="" />
        <p>Coat of arms</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      countrySearch: "",
      visible: false,
      countryFound: false,
      onMainSite: true,

      commonName: "",
      officialName: "",
      nativeName: "",
      capital: "",
      continent: "",
      subregion: "",
      population: "",

      // images
      flag: "",
      coatOfArms: "",
    };
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
        this.nativeName = output[0].name.nativeName;

        this.capital = output[0].capital[0];
        this.continent = output[0].continents[0];
        this.subregion = output[0].subregion;
        this.population = output[0].population;

        // clears the input after searching is done
        this.countrySearch = "";

        // images
        this.flag = output[0].flags.svg;
        this.coatOfArms = output[0].coatOfArms.svg;

        // if country found show data
        this.visible = true;
        this.countryFound = false;
        this.onMainSite = false;
      } catch (error) {
        // if country not found show error
        console.log(error);
        this.visible = false;
        this.countryFound = true;
        this.onMainSite = false;
      }
    },
  },
};
</script>

<style>
.countries {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.countries__form {
  margin: var(--size--medium);
}

.countries__input {
  border: 2px solid black;
  border-radius: 2rem;
  padding: 1rem;
  width: 40rem;
  font-size: var(--font--caption);
}

.countries__info {
  margin: var(--size--big);
  font-size: var(--font--body);
  color: rgba(0, 0, 0, 0.822);
}

.countries__data {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin: 0 var(--size--big);
  background: #e7e7e7;
  border: 0.8rem solid #d4d4d4;
  padding: var(--size--medium) var(--size--small);
  width: 70rem;
}

.data__facts h1 {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: var(--size--big) 0;
  border-bottom: 4px solid black;
  font-size: var(--font--heading);
  max-width: 30rem;
}

p {
  font-size: var(--font--caption);
}

.data__facts h2 {
  margin: var(--size--big) 0;
  font-size: var(--font--body);
  max-width: 30rem;
}

.data__images {
  display: flex;
  flex-direction: column;
}

.data__images img {
  max-width: 20rem;
  max-height: 25rem;
  margin-top: var(--size--big);
}

.data__images p {
  margin-top: 2rem;
}

.countries__error {
  font-size: var(--font--body);
  color: red;
  margin: var(--size--big) 0;
}
</style>