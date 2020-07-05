<style>
  .grid {
    flex: 1;
    display: grid;
    grid-template-columns: 1fr 1fr;
    justify-items: center;
    align-items: center;
  }

  .inputs {
    display: flex;
    flex-direction: column;
  }

  .range-container {
    display: flex;
    align-items: center;
    padding: 1em;
  }

  label {
    font-size: 1.5em;
    width: 250px;
  }

  input {
    padding: .5em .25em;
    margin: 0 .5em;
  }

  .number {
    width: 60px;
  }

  .result {
    font-size: 2em;
  }


</style>

<script>
  import {bignumber, combinations, pow, subtract, divide, multiply, chain, round } from 'mathjs';
  let probability;

  let numPeople = 10;
  let rate = 5;
  let numInfected = 1;

  const calculateProbability = (trials, successes, successRate) => {
    const t = bignumber(trials);
    const s = bignumber(successes);
    const r = divide(bignumber(successRate), 100)

    return chain(combinations(t, s))
    .multiply(pow(r, s))
    .multiply(pow(subtract(1,r), subtract(t, s)))
    .done();
  }

  $: probability = calculateProbability(numPeople, numInfected, rate);
  $: console.log(numPeople, numInfected, divide(rate, 100), probability);

  $: if(numInfected > numPeople) numInfected = numPeople;
</script>

<svelte:head>
	<title>Covid Odds</title>
</svelte:head>

<div class="grid">
  <div class="inputs">
    <div class="range-container">
      <label for="people"> Number of People: </label>
      <input class="number" bind:value={numPeople} type="number" min="1" max="1000" id="people">
      <input class="range" bind:value={numPeople} type="range" min="1" max="1000" id="people">
    </div>

    <div class="range-container">
      <label for="rate"> Background Rate: </label>
      <input class="number" bind:value={rate} type="number" min="0" max="100" id="people">%
      <input class="range" bind:value={rate} type="range" min="0" max="100" id="rate">
    </div>

    <div class="range-container">
      <label for="infected"> Number of in Infected: </label>
      <input class="number" bind:value={numInfected} type="number" min="0" max={numPeople} id="people">
      <input class="range" bind:value={numInfected} type="range" min="0" max={numPeople} id="infected">
    </div>
  </div>
  <div class="result">
    {round(multiply(probability, 100), 2)}%
  </div>
</div>

