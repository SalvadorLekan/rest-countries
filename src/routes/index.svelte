<script context="module">
  export async function preload() {
    let res = await this.fetch("https://restcountries.eu/rest/v2/all");

    let countries = await res.json();

    return { countries };
  }
</script>

<script lang="ts">
  export let countries: {
    name: string;
    topLevelDomain: string[];
    alpha2Code: string;
    alpha3Code: string;
    callingCodes: string[];
    capital: string;
    altSpellings: string[];
    region: string;
    subregion: string;
    population: number;
    latlng: [number, number];
    demonym: string;
    area: number;
    gini: number;
    timezones: string[];
    borders: string[];
    nativeName: string;
    numericCode: string;
    currencies: { code: string; name: string; symbol: string }[];
    languages: {
      iso639_1: string;
      iso639_2: string;
      name: string;
      nativeName: string;
    }[];
    translations: {
      de: string;
      es: string;
      fr: string;
      ja: string;
      it: string;
      br: string;
      pt: string;
      nl: string;
      hr: string;
      fa: string;
    };
    flag: string;
    regionalBlocs: {
      acronym: string;
      name: string;
      otherAcronyms: unknown[];
      otherNames: unknown[];
    }[];
    cioc: string;
  }[];
  let search: string = "";
  let continent = "";

  $: buttonText = continent || "Filter by Region";

  const regions = ["Africa", "America", "Asia", "Europe", "Oceania"];
</script>

<svelte:head>
  <title>World Countries</title>
</svelte:head>

<div id="landing">
  <section id="cc">
    <div class="search">
      <label for="search"
        ><svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-search"
          viewBox="0 0 16 16"
        >
          <path
            d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"
          />
        </svg></label
      >
      <input
        type="text"
        bind:value={search}
        placeholder="Search for a country..."
      />
    </div>
    <div class="v">
      <div class="dqpl-field-wrap">
        <button
          >{buttonText}
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-chevron-down"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z"
            />
          </svg></button
        >
        {#each regions as region}
          <button
            class:active={region === continent}
            on:click={() => (continent = region)}
            >{region}
          </button>
        {/each}
      </div>
    </div>
  </section>
</div>

<section id="list">
  {#each countries as country}
    {#if country.region.includes(continent) && country.name
        .toLowerCase()
        .includes(search.toLowerCase())}
      <a sapper:prefetch href={`/countries/${country.name.split(" ")[0]}`}>
        <img src={country.flag} width="265" height="170" alt={country.name} />

        <article>
          <h3>{country.name}</h3>
          <p><span>Population: </span> {country.population.toLocaleString()}</p>
          <p><span>Region: </span> {country.region}</p>
          <p><span>Capital: </span> {country.capital}</p>
        </article>
      </a>
    {/if}
  {/each}
</section>

<style>
  #cc {
    padding-block: 30px;
    display: flex;
    flex-wrap: wrap;
    row-gap: 3ch;
    align-items: flex-start;
    position: relative;
    justify-content: space-between;
  }
  a {
    color: inherit;
    display: block;
    text-decoration: inherit;
    width: 265px;
    border-radius: 1ch;
    overflow: hidden;
    box-shadow: 0px 0px 5px #888;
    background-color: var(--bg-secondary);
  }
  #list {
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
    row-gap: 7ch;
  }

  article {
    padding-inline: 20px;
    padding-block-end: 30px;
  }
  article p span {
    font-weight: 600;
  }
  input {
    color: inherit;
    background-color: inherit;
    border: none;
    flex: auto;
    font-size: inherit;
  }
  :focus {
    outline: none;
  }
  #landing div.search {
    display: grid;
    grid-template-columns: auto 1fr;
    font-size: 16px;
    border-radius: 1ch;
    padding: 15px 30px;
    background: var(--bg-secondary);
    gap: 2ch;
    box-shadow: 0px 0px 5px #888;
    align-content: center;
    max-width: 480px;
  }
  #landing label {
    display: grid;
    place-content: center;
  }
  ::placeholder {
    color: inherit;
  }
  @media (min-width: 1200px) {
    #cc {
      padding: 30px 0;
    }
    #list {
      justify-content: space-between;
    }
  }
  .dqpl-field-wrap {
    position: absolute;
    width: 200px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    box-shadow: 0px 0px 5px #888;
    right: 0;
    overflow: hidden;
    border-radius: 1ch;
  }
  .dqpl-field-wrap button {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: var(--bg-secondary);
    border: none;
    padding: 2ch 3ch;
    width: 200px;
    color: inherit;
    font-weight: 600;
  }
  .dqpl-field-wrap button:nth-of-type(1) {
    border-radius: 1ch;
    position: relative;
    z-index: 2;
  }
  .dqpl-field-wrap:focus-within button:nth-of-type(1) {
    margin-bottom: 5px;
  }
  .dqpl-field-wrap button:not(:nth-of-type(1)) {
    position: absolute;
  }
  .dqpl-field-wrap:focus-within button:not(:nth-of-type(1)) {
    position: relative;
  }
  .v {
    position: relative;
    width: 200px;
    height: 5ch;
  }
  button.active,
  button:not(:nth-of-type(1)):focus {
    background-color: lightblue;
  }
</style>
