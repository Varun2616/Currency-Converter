<script>
  import {onMount} from 'svelte';

  let currencies = [];
  let fromCurrency = 'USD';
  let toCurrency = 'EUR';
  let amount = 1;
  let convertedAmount = 0;
  let exchangeRate = 0;
	
	async function fetchExchangeRates() {
    const response = await fetch('https://api.exchangerate-api.com/v4/latest/' + fromCurrency);
    const data= await response.json();
    currencies= Object.keys(data.rates);
    exchangeRate= data.rates[toCurrency];
    convertedAmount= (amount*exchangeRate).toFixed(2);
  }
	//update the fetch rate
  $: fromCurrency, toCurrency, amount, fetchExchangeRates();

  onMount(fetchExchangeRates);  
</script>

<style>
  .currency-converter {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }
  select,input{
    margin: 5px;
    padding: 8px;
  }
</style>

<div class="currency-converter">
  <h2>Currency Converter</h2>

  <div>
    <label for="from">From:</label>
    <select bind:value={fromCurrency} id="from">
      {#each currencies as currency}
        <option value={currency}>{currency}</option>
      {/each}
    </select>
  </div>

  <div>
    <label for="to">To:</label>
    <select bind:value={toCurrency} id="to">
      {#each currencies as currency}
        <option value={currency}>{currency}</option>
      {/each}
    </select>
  </div>

  <div>
    <label for="amount">Amount:</label>
    <input type="number" id="amount" bind:value={amount} />
  </div>
    <div class="result">
      <p>{amount} {fromCurrency} = {convertedAmount} {toCurrency}</p>
    </div>
  
</div>

