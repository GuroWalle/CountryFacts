<!-- Gotten help from: https://www.youtube.com/watch?v=5Uxe_MNd6go&list=PLgiAmmJALlgzvpogJwhFytLmh6tI-TQSN&index=13&ab_channel=SomTeaCodes -->

<template>
  <div class="countries">
    <!-- .prevent prevnts the site from refreshing -->
    <form v-on:submit.prevent="fetchNewCountry">
      <input
        type="text"
        placeholder="Search for a country..."
        v-model="countrySearch"
        autocomplete="off"
        class="countries__search-bar"
      />
    </form>
    <button class="countries__search--button" @click="fetchNewCountry">
      Search
    </button>

    <p class="countries__app-info" v-if="onMainSite">
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
        countryFound: false,
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
          this.capital = output[0].capital[0];
          this.region = output[0].region;
          this.subregion = output[0].subregion;
          this.population = output[0].population;
          this.flag = output[0].flags.svg;
          this.coatOfArms = output[0].coatOfArms.svg;

          // Native name
          // find the name of all properites in the object
          for (var propertyName in output[0].name.nativeName) {
            // Show the name of the property and use it to retrieve the object itself (and not just the name)
            this.nativeName =
              propertyName +
              ": " +
              output[0].name.nativeName[propertyName].common;
          }

          // Clears the input after pressing enter / submitting
          this.countrySearch = "";

          // If country found show data
          this.visible = true;
          this.countryFound = false;
          this.onMainSite = false;
        } catch (error) {
          // If country not found show error
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
    width: 100%;
    height: 100vh;
  }

  .countries__search-bar {
    padding: 0.7rem 10rem;
    border: 0.3rem solid black;
    border-radius: 2rem;
    margin: var(--size-big) 0 var(--size-medium) 0;
    font-size: var(--font-body);
  }

  .countries__search--button {
    padding: 0.7rem 3rem;
    border-radius: 2rem;
    border: 0.2rem solid black;
    font-size: var(--font-caption);
    color: black;
    background: lightgrey;
  }

/* shows if onMainSite = true */
  .countries__app-info {
    margin: var(--size-big) 0;
    text-align: center;
    font-size: var(--font-body);
  }

/* shows if countryFound = false */
  .countries__error {
    margin: var(--size-big) 0;
    text-align: center;
    font-size: var(--font-body);
    color: red;
  }

/* shows if visible = true */
  .countries__data {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    width: 80%;
    padding: 3rem;
    border: 0.8rem solid #d4d4d4;
    margin: var(--size-medium);
    background: #e7e7e7;
  }

  .data__facts {
    max-width: 30rem;
    width: 100%;
  }

  .data__facts h1 {
    display: flex;
    flex-direction: row;
    justify-content: center;
    border-bottom: 0.3rem solid black;
    font-size: var(--font-heading);
  }

  p {
    font-size: var(--font-caption);
  }

  .data__facts h2 {
    margin: var(--size-big) 0;
    font-size: var(--font-body);
  }

  .data__images {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .data__images img {
    max-width: 23rem;
    max-height: 25rem;
    margin-top: var(--size-big);
  }

  .data__images p {
    margin-top: 2rem;
    max-width: 23rem;
  }

  @media screen and (max-width: 500px){
    .countries__search-bar {
      padding: 0.9rem 4rem;
      border: 0.2rem solid black;
      font-size: var(--font-caption);
    }

  /* shows if onMainSite = true */
    .countries__app-info {
      font-size: var(--font-caption);
    }

  /* shows if visible = true */
    .countries__data {
      flex-direction: column;
      width: 100%;
      padding: var(--size-small) var(--size-small);
      border: 0.4rem solid #d4d4d4;
      margin: var(--size-medium);
    }

    .data__facts h1 {
      margin: var(--size-small) 0;
      font-size: var(--font-heading);
    }

    .data__images img {
      margin-top: var(--size-medium);
    }
  }

</style>