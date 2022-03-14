<!-- Gotten help from: https://www.youtube.com/watch?v=5Uxe_MNd6go&list=PLgiAmmJALlgzvpogJwhFytLmh6tI-TQSN&index=13&ab_channel=SomTeaCodes -->

<template>
  <div class="countries">
    <!-- .prevent prevnts the site from refreshing -->
    <form class="countries__form" v-on:submit.prevent="fetchNewCountry">
      <input
        type="text"
        placeholder="Search for a country..."
        v-model="countrySearch"
        autocomplete="off"
        class="countries__input"
      />
    </form>

    <p class="countries__app-info" v-if="onMainSite">
      Find basic facts about every country in the world!
    </p>
    <p class="countries__error" v-if="noCountryFound">No country found...</p>

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
          {{ region }}, {{ subregion }}
        </h2>
        <h2>
          <p>Population:</p>
          {{ population }}
        </h2>
      </div>
      <div class="data__images">
        <img :src="flag" alt="" />
        <p>{{ commonName }}'s flag</p>
        <img :src="coatOfArms" alt="" />
        <p>{{ commonName }}'s coat of arms</p>
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
      noCountryFound: false,
      onMainSite: true,

      // Text
      commonName: "",
      officialName: "",
      nativeName: "",
      capital: "",
      region: "",
      subregion: "",
      population: "",

      // Images
      flag: "",
      coatOfArms: "",
    };
  },

  methods: {
    async fetchNewCountry() {
      console.log(this.countrySearch);
      // Fetches data from the api with the same name as searched
      const url = `https://restcountries.com/v3.1/name/${this.countrySearch}`;
      const result = await fetch(url);

      try {
        const output = await result.json();

        this.commonName = output[0].name.common;
        this.officialName = output[0].name.official;
        // I have trouble with this one since nativeName is an object and not an array
        this.nativeName = output[0].name.nativeName.common;
        this.capital = output[0].capital[0];
        this.region = output[0].region;
        this.subregion = output[0].subregion;
        this.population = output[0].population;
        this.flag = output[0].flags.svg;
        this.coatOfArms = output[0].coatOfArms.svg;

        // Clears the input after pressing enter / submitting
        this.countrySearch = "";

        // If country found show data
        this.visible = true;
        this.noCountryFound = false;
        this.onMainSite = false;
      } catch (error) {
        // If country not found show error
        console.log(error);
        this.visible = false;
        this.noCountryFound = true;
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

/* shows if onMainSite = true */
.countries__app-info {
  margin: var(--size--big);
  font-size: var(--font--body);
  color: rgba(0, 0, 0, 0.822);
}

/* shows if noCountryFound = false */
.countries__error {
  font-size: var(--font--body);
  color: red;
  margin: var(--size--big) 0;
}

/* shows if visible = true */
.countries__data {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  margin: 0 var(--size--big);
  background: #e7e7e7;
  border: 0.8rem solid #d4d4d4;
  padding: var(--size--medium) var(--size--small);
  width: 80%;
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
  align-items: center;
}

.data__images img {
  max-width: 23rem;
  max-height: 25rem;
  margin-top: var(--size--big);
}

.data__images p {
  margin-top: 2rem;
  max-width: 23rem;
}

@media screen and (max-width: 800px) {
  .countries {
    flex-direction: column;
  }

  .countries__input {
    width: 20rem;
  }

  /* shows if onMainSite = true */
  .countries__app-info {
    font-size: var(--font--caption);
    margin: var(--size--medium) var(--size--medium);
  }

  /* shows if noCountryFound = false */
  .countries__error {
    font-size: var(--font--caption);
    margin: var(--size--medium) var(--size--medium);
  }

  /* shows if visible = true */
  .countries__data {
    flex-direction: column;
    margin: 0 var(--size--small);
    border: none;
    padding: var(--size--small) var(--size--small);
    border-top: 0.4rem solid #d4d4d4;
    border-bottom: 0.4rem solid #d4d4d4;
    width: 100%;
  }

  .data__facts h1 {
    margin: var(--size--small) 0;
    font-size: var(--font--heading); /* NOT SURE YET */
  }

  .data__images img {
    margin-top: var(--size--medium);
  }
}
</style>