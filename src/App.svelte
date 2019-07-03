<script>
	import Calculator from './Calculator.svelte'
	
	let usdToIlsPromise = fetch('https://api.exchangeratesapi.io/latest?base=USD')
		.then(r => r.json())
		.then(data => data.rates.ILS)
</script>

<style>
	div {
		padding: 10px;
		max-width: 400px;
		margin: 0 auto;
		background-color: rgb(254, 255, 240);
		border-radius: 2px;
		box-shadow: 0 0 3px 0 black;
	}
	h1 {
		color: purple;
		text-align: center;
	}

	pre {
		text-align: center;
		margin-top: 150px;
	}
</style>

<div>
	<h1>Money Calculator</h1>
	{#await usdToIlsPromise}
		<pre>fetching data....</pre>
	{:then usdToIls}
		<Calculator conversionRate={usdToIls} currencyLabel="₪" />
	{/await}
</div>
