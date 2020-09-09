<script>

</script>

<section class="ip-tracker">
  <div class="container">
    <h1 class="t-h1 t-white ip-tracker__heading">IP Address Tracker</h1>
    <div class="ip-tracker__search-box">
      <div class="input-group">
        <label class="sr-only" for="ip-address">IP address or domain</label>
        <input class="input-group__input" type="text" id="ip-address" name="ip-address" placeholder="Search for any IP address or domain">
      </div>
      <button class="button button--search">
        <span class="sr-only">search</span>
        <svg xmlns="http://www.w3.org/2000/svg" width="11" height="14"><path fill="none" stroke="#FFF" stroke-width="3" d="M2 1l6 6-6 6"/></svg>
      </button>
    </div>

    <div class="ip-tracker__results">
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">IP Address</h2>
        <p class="ip-tracker__result__body t-body t-dark-gray">192.212.174.101</p>
      </div>
      <div class="ip-tracker__result__divider"></div>
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">Location</h2>
        <p class="ip-tracker__result__body t-body t-dark-gray">Brooklyn, NY 10001</p>
      </div>
      <div class="ip-tracker__result__divider"></div>
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">Timezone</h2>
        <p class="ip-tracker__result__body t-body t-dark-gray">UTC -05:00</p>
      </div>
      <div class="ip-tracker__result__divider"></div>
      <div class="ip-tracker__result__box">
        <h2 class="ip-tracker__result t-h2 t-mid-gray">ISP</h2>
          <p class="ip-tracker__result__body t-body t-dark-gray">SpaceX<br>Starlink</p>
      </div>
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
    letter-spacing: 0;
    line-height: 2.2rem;
    color: var(--color-dark-gray);
    font-weight: var(--weight-regular);
  }

  .input-group__input::placeholder {
    opacity: .50;
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
      display: inline-block;
      background-color: var(--color-black);
    }
  }
</style>