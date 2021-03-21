<script context="module" lang="ts">
  export async function preload({ params }: { params: { country: string } }) {
    const { country } = params;
    let res = await this.fetch(
      "https://restcountries.eu/rest/v2/name/" + country
    );

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
</script>

<svelte:head>
  <title>{countries[0].name}</title>
</svelte:head>

<a href="/"
  ><svg
    xmlns="http://www.w3.org/2000/svg"
    width="16"
    height="16"
    fill="currentColor"
    class="bi bi-arrow-left-short"
    viewBox="0 0 16 16"
  >
    <path
      fill-rule="evenodd"
      d="M12 8a.5.5 0 0 1-.5.5H5.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L5.707 7.5H11.5a.5.5 0 0 1 .5.5z"
    />
  </svg><span>Back</span></a
>
<section>
  <img src={countries[0].flag} alt="{countries[0].name} flag" />
  <article>
    <h2>{countries[0].name}</h2>
    <div>
      <div>
        <p><span>Native Name: </span>{countries[0].nativeName}</p>
        <p>
          <span>Population: </span>{countries[0].population.toLocaleString()}
        </p>
        <p><span>Region: </span>{countries[0].region}</p>
        <p><span>Sub Region: </span>{countries[0].subregion}</p>
        <p><span>Capital: </span>{countries[0].capital}</p>
      </div>
      <div>
        <p><span>Top Level Domain: </span>{countries[0].topLevelDomain[0]}</p>
        <p><span>Currencies: </span>{countries[0].currencies[0].name}</p>
        <p>
          <span>Languages: </span>
          {#each countries[0].languages as language}
            <span>{language.name}</span>
          {/each}
        </p>
      </div>
    </div>
    <p>
      <span>Border Countries: </span>
      {#each countries[0].borders as border}
        <span class="border">{border}</span>
      {/each}
    </p>
  </article>
</section>

<style>
  .border,
  a {
    display: inline-grid;
    grid-template-columns: auto 1fr;
    font-size: 16px;
    border-radius: 1ch;
    padding: 12px 24px;
    background: var(--bg-secondary);

    box-shadow: 0px 0px 5px #888;
    align-content: center;
    color: inherit;
    text-decoration: none;
  }
  a {
    gap: 2ch;
    margin-block: 50px;
    width: 70px;
  }
  .border {
    margin: 5px;
  }
  img {
    width: 100%;
    max-width: 560px;
  }
  section {
    display: flex;
    align-items: center;
    gap: 5ch;
    flex-wrap: wrap;
  }
  article {
    flex: auto;
  }
  article > div {
    display: flex;
    flex-wrap: wrap;
  }
  p span:nth-of-type(1) {
    font-weight: 600;
    font-size: large;
  }
  h2 {
    font-size: xx-large;
    font-weight: 800;
  }
  @media (min-width: 1200px) {
    section {
      flex-wrap: nowrap;
    }
  }
</style>
