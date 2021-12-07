<script>
  import Modal, { getModal } from "./Modal.svelte";

  let amount;
  let baseCoin;
  let convertCoin;
  let result;
  function handleSubmit(e) {
    conversion(amount, baseCoin, convertCoin);
  }
  async function conversion(amount, baseCoin, convertCoin) {
    const call = await fetch(
      "http://localhost:1337/api/my-crypto-converter/convert",
      {
        method: "POST",
        body: JSON.stringify({
          baseCoin: baseCoin,
          compareCoin: convertCoin,
          amount: amount,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      }
    );
    result = await call.json();
    //opens modal
    getModal().open();
  }
</script>

<div class="container">
  <h1>Crypto converter</h1>
  <form id="surveyForm" class="mt-4" on:submit|preventDefault={handleSubmit}>
    <div class="form-group">
      <label for="baseCoin">Choose base currency:</label>
      <select name="baseCoin" bind:value={baseCoin}>
        <option value="BTC">Bitcoin</option>
        <option value="ETH">Ethereum</option>
        <option value="XRP">Ripple</option>
        <option value="audi">Audi</option>
      </select>
    </div>
    <div class="form-group">
      <label for="compareCoin">Choose currency to convert to:</label>
      <select name="compareCoin" bind:value={convertCoin}>
        <option value="BTC">Bitcoin</option>
        <option value="ETH">Ethereum</option>
        <option value="XRP">Ripple</option>
        <option value="audi">Audi</option>
      </select>
    </div>
    <div class="form-group">
      <input
        type="num"
        class="form-control"
        placeholder="Enter amount in coin"
        bind:value={amount}
        required
      />
    </div>
    <button class="btn btn-full">Convert</button>
  </form>
  <Modal>
    <h4>Success!!</h4>
    <h3>
      <small>You will get</small>
      {result} <small>in</small>
      {convertCoin}
    </h3>
  </Modal>
</div>
<!-- the modal without an `id` -->

<style>
  .container {
    max-width: 1200px;
    margin: 0 auto;
    margin-top: 8rem;
  }
  h2 {
    margin-top: 0;
  }
  .form-group > *,
  .btn-full {
    width: 100%;
  }
  .form-control,
  .btn-full {
    border-radius: 3px;
  }
  .submitted input:invalid {
    border: 1px solid #c00;
  }
  .submitted input:focus:invalid {
    outline: 1px solid #c00;
  }
  .error-alert {
    border: 1px solid #c00 !important;
    padding: 6px;
    text-align: center;
    color: #c00;
    border-radius: 3px;
  }
</style>
