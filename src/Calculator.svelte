<script>
    
    export let conversionRate = 1
    export let currencyLabel = '$'

    let type = 'options'
    let numberOfOptions = 200
    let excercisePrice = 100
    let stockPrice = 200
    let total = 0

    const fruitTax = 0.33 + 0.12 + 0.03
	const honTax = 0.25 + 0.03

	$: if (type === 'rsu') {
        const bigTax = excercisePrice * fruitTax
		const smallTax = (stockPrice - excercisePrice) * honTax

		total = (stockPrice - bigTax - smallTax) * numberOfOptions * conversionRate
    } else {
        total = (stockPrice - excercisePrice) * numberOfOptions * (1 - honTax) * conversionRate
    }

    const format = n => n.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,')

    const autoSelect = e => e.target.select()

    let input

    $: input && input.select()
</script>

<style>
    label, p {
        display: grid;
        width: 100%;
        grid-template-columns: 1fr 1fr;
        grid-auto-rows: 40px;
        grid-column-gap: 10px;
        align-items: center;
    }

    label > :first-child, p > :first-child {
        justify-self: end;
    }

    label > :nth-child(2) {
        justify-self: start;
        margin: 0;
        width: 100px;
    }

    p {
        margin: 30px 0;
        font-size: 1.7em;
    }

    .happy {
        color: rgb(71, 159, 71);
        font-weight: bold;
    }

    pre {
        margin-top: 20px;
        text-align: center;
    }
</style>

<label>
    <span>Select Type:</span>
    <select bind:value={type}>
        <option value="options">
            Options
        </option>
        <option value="rsu">
            RSUs
        </option>
    </select>
</label>

<label>
    <span>Number of {type === 'options' ? 'Options:' : 'RSUs:'}</span>
    <input type="number" bind:value={numberOfOptions} on:focus={autoSelect} />
</label>

<label>
    <span>{type === 'options' ? 'Excercise' : 'Average'} Price $:</span>
    <input type="number" bind:value={excercisePrice}  on:focus={autoSelect} />
</label>

<label>
    <span>Stock Price $:</span>
    <input type="number" bind:value={stockPrice}  on:focus={autoSelect} />
</label>

<p>
    <span>Your score is:</span>
    <span class="happy">{format(total)}{currencyLabel}</span>
</p>

<pre>$1.00 = {currencyLabel}{format(conversionRate)}</pre>