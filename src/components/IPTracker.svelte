<script>
  import { onMount } from 'svelte';
  import { createEventDispatcher } from 'svelte';
  import { Address6, Address4 } from 'ip-address';
  import isValidDomain from 'is-valid-domain';
  import Loader from './Loader.svelte';

  const dispatch = createEventDispatcher();

  let ip;
  let error = false;
  let loaded = false;
  let isFetched = false;
  
  let resultIp;
  let resultLocation;
  let resultTimeZone;
  let resultISP;

  let debug = false;

  function updateMapLocation(updatedResult) {
    const result = {
      lat: updatedResult.location.lat,
      lng: updatedResult.location.lng
    };
    dispatch('updateResult', { result });
  }

  function updateIPInfo(updatedResult) {
    resultIp = updatedResult.ip;
    resultLocation = `${updatedResult.location.city}, ${updatedResult.location.region} ${updatedResult.location.postalCode}`;
    resultTimeZone = updatedResult.location.timezone;
    resultISP = updatedResult.isp;
  }
  
  async function search() {
    if (ip) {
      const addr6 = new Address6(ip);
      const addr4 = new Address4(ip);
      if (addr6.isValid() || addr4.isValid()) {
        isFetched = false;
        const updatedResult = await getGeoInfo(ip, true);
        updateMapLocation(updatedResult);
        updateIPInfo(updatedResult);
        isFetched = true;
        ip = '';
      } else if (isValidDomain(ip)) {
        isFetched = false;
        const updatedResult = await getGeoInfo(ip, false);
        updateMapLocation(updatedResult);
        updateIPInfo(updatedResult);
        isFetched = true;
        ip = '';
      } else {
        ip = '';
        error = true;
        return;
      }
    } else {
      error = true;
    }
  }

  async function getGeoInfo(ip, isIp) {
    if (isIp) {
      const clientRes = await fetch(`https://geo.ipify.org/api/v1?apiKey=at_Nr1EYCfMNqXK1E50NoR5TnhQqFlRV&ipAddress=${ip}`);
      return await clientRes.json();
    } else {
      const clientRes = await fetch(`https://geo.ipify.org/api/v1?apiKey=at_Nr1EYCfMNqXK1E50NoR5TnhQqFlRV&domain=${ip}`);
      return await clientRes.json();
    }
  }

  const removeError = () => error = false;

  onMount(async () => {
    if (debug) {
      resultIp = '192.212.174.101';
      resultLocation = 'Brooklyn, NY 10001';
      resultTimeZone = '-05:00';
      resultISP = `SpaceX<br>Starlink`;
      isFetched = true;
    } else {
      const initRes = await fetch(`https://api.ipify.org?format=json`);
      const initResJson = await initRes.json();
      const clientRes = await fetch(`https://geo.ipify.org/api/v1?apiKey=at_Nr1EYCfMNqXK1E50NoR5TnhQqFlRV&ipAddress=${initResJson.ip}`);
      const clientInfo = await clientRes.json();
      updateMapLocation(clientInfo);
      updateIPInfo(clientInfo);
      isFetched = true;
    }
  });
</script>

<section class="ip-tracker">
  <div class="container">
    <h1 class="t-h1 t-white ip-tracker__heading">IP Address Tracker</h1>
    <div class="ip-tracker__search-box">
      <div class="input-group">
        <label class="sr-only" for="ip-address">IP address or domain</label>
        <input on:focus={removeError} bind:value={ip} class="input-group__input{error ? ' error': ''}" type="text" id="ip-address" name="ip-address" placeholder="{error ? 'Invalid input' : 'Search for any IP address or domain'}">
      </div>
      <button on:click={search} class="button button--search">
        <span class="sr-only">search</span>
        <svg xmlns="http://www.w3.org/2000/svg" width="11" height="14"><path fill="none" stroke="#FFF" stroke-width="3" d="M2 1l6 6-6 6"/></svg>
      </button>
    </div>
    <div class="ip-tracker__results">
      {#if isFetched}
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">IP Address</h2>
        <p class="ip-tracker__result__body t-body t-dark-gray">{resultIp}</p>
      </div>
      <div class="ip-tracker__result__divider"></div>
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">Location</h2>
        <p class="ip-tracker__result__body t-body t-dark-gray">{resultLocation}</p>
      </div>
      <div class="ip-tracker__result__divider"></div>
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">Timezone</h2>
        <p class="ip-tracker__result__body t-body t-dark-gray">UTC {resultTimeZone}</p>
      </div>
      <div class="ip-tracker__result__divider"></div>
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">ISP</h2>
          <p class="ip-tracker__result__body t-body t-dark-gray">{@html resultISP}</p>
      </div>
      {:else}
      <div class="loading">
        <Loader/>
        <h3 class="t-h3">Searching, please wait...</h3>
      </div>
      {/if}
    </div>
  </div>
</section>

<style>
  .ip-tracker {
    padding: 0 2.4rem;
    background-size: cover;
    background-position: center center;
    background-image: url(/assets/images/pattern-bg.png);
  }
  .ip-tracker > .container {
    padding-top: 2.6rem;
    position: relative;
    padding-bottom: 15.7rem;
  }
  @media (min-width: 768px) {
    .ip-tracker > .container {
      margin: 0 auto;
      max-width: 111rem;
      padding-top: 3.3rem;
      padding-bottom: 12.8rem;
    }
  }

  .ip-tracker__heading {
    text-align: center;
    margin-bottom: 2.9rem;
  }
  @media (min-width: 768px) {
    .ip-tracker__heading {
      margin-bottom: 3.1rem;
    }
  }

  .input-group {
    width: 100%;
  }

  .input-group__input {
    width: 100%;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 1.8rem 2.4rem;
    border-radius: 1.5rem 0 0 1.5rem;
  }

  .input-group__input,
  .input-group__input::placeholder {
    font-size: 1.8rem;
    letter-spacing: .26px;
    line-height: 2.2rem;
    color: var(--color-dark-gray);
    font-weight: var(--weight-regular);
  }

  .input-group__input::placeholder {
    opacity: .50;
  }
  .input-group__input.error::placeholder {
    opacity: 1;
    color: #fc5151;
  }

  .ip-tracker__search-box {
    display: flex;
    margin: 0 auto;
    max-width: 55.5rem;
    flex-flow: row nowrap;
  }
  
  .ip-tracker__results {
    z-index: 999;
    left: 0;
    right: 0;
    top: 16.7rem;
    display: flex;
    position: absolute;
    align-items: center;
    border-radius: 1.5rem;
    flex-flow: column nowrap;
    padding: 2.6rem 2.4rem 2.4rem;
    background-color: var(--color-white);
    box-shadow: 0 5rem 5rem -2.5rem rgba(0,0,0,0.10);
  }
  @media (min-width: 768px) {
    .ip-tracker__results {
      top: 20rem;
      flex-flow: row nowrap;
      align-items: flex-start;
      padding: 3.7rem 3.2rem 3.6rem;
      justify-content: space-between;
    }
  }

  .ip-tracker__result {
    text-align: center;
  }
  @media (min-width: 768px) {
    .ip-tracker__result {
      text-align: left;
    }
  }

  .ip-tracker__result__box {
    width: 100%;
    text-align: center;
  }
  @media (min-width: 768px) {
    .ip-tracker__result__box {
      width: 21%;
      text-align: left;
      max-width: 21.3rem;
    }
  }

  .ip-tracker__result__box:not(:last-of-type) {
    margin-bottom: 2.4rem;
  }
  @media (min-width: 768px) {
    .ip-tracker__result__box:not(:last-of-type) {
      margin-bottom: 0;
    }

  }

  .ip-tracker__result__body {
    margin-top: .7rem;
    word-break: break-word;
  }
  @media (min-width: 768px) {
    .ip-tracker__result__body {
      margin-top: 1.3rem;
    }
  }

  .ip-tracker__result__divider {
    display: none;
  }
  @media (min-width: 768px) {
    .ip-tracker__result__divider {
      width: .1rem;
      opacity: .15;
      height: 7.5rem;
      align-self: center;
      display: inline-block;
      background-color: var(--color-black);
    }
  }

  .loading {
    width: 100%;
    display: flex;
    align-items: center;
    flex-flow: column nowrap;
  }
</style>