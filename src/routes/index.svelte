<script lang="ts">
  const gearSpeeds = [3, 4, 5, 6, 7, 8, 9, 10] as const;
  const minGearRatio = 0.48;
  const maxGearRatio = 6;
  const interval = 0.01;

  let first = 3;
  let last = 1;
  let shape = 1;
  let count = 6;
  let result: string[] = [];

  $: actualShape = shape * count;
  $: if (first < last) {
    last = first;
  }

  function displayCalculated() {
    const magicValue = findMagicValue();
    if (magicValue === null) {
      alert("Shape is wrong");
      return;
    }
    result = calculate(magicValue).map((value) => round(value).toFixed(2));
  }

  function findMagicValue() {
    let min = 0;
    let max = 10;
    let result: number;

    while (min < max) {
      result = (min + max) / 2;
      const calculated = calculate(result);
      const calculatedLast = calculated[calculated.length - 1];

      if (calculatedLast > last) {
        max = result;
      } else if (calculatedLast < last) {
        min = result;
      } else {
        return result;
      }
    }
    return null;
  }

  function calculate(magicValue: number) {
    let gearRatio = first;
    const result = [round(gearRatio)];

    for (let i = 1; i < count; i++) {
      const x = i / (count - 1);
      gearRatio -= gearRatio / (actualShape * x + magicValue);
      result.push(round(gearRatio));
    }
    return result;
  }

  function round(value: number) {
    return Math.round(value * 100) / 100;
  }
</script>

<svelte:head>
  <title>Gear Ratio Calculator</title>
</svelte:head>

<div>
  <div>
    <label class="slider">
      <span class="left-label">First</span>
      <input
        type="range"
        min={minGearRatio}
        max={maxGearRatio}
        step={interval}
        bind:value={first}
      />
      <span class="right-label">{first.toFixed(2)}</span>
    </label>
  </div>

  <div>
    <label class="slider">
      <span class="left-label">Last</span>
      <input
        type="range"
        min={minGearRatio}
        max={maxGearRatio}
        step={interval}
        bind:value={last}
      />
      <span class="right-label">{last.toFixed(2)}</span>
    </label>
  </div>

  <div>
    <label class="slider">
      <span class="left-label">Low-rev</span>
      <input type="range" min="0" max="2" step="0.1" bind:value={shape} />
      <span class="right-label">High-rev</span>
    </label>
  </div>

  <div class="etc">
    <select bind:value={count}>
      {#each gearSpeeds as gear}
        <option value={gear}>{gear} Speed</option>
      {/each}
    </select>
    <button on:click={displayCalculated}>Calculate</button>
  </div>

  <div class="result">
    {#each result as value, index}
      <div>{index + 1}: {value}</div>
    {/each}
  </div>
</div>

<style>
  .slider {
    display: flex;
    gap: 1rem;
  }

  .slider input {
    width: 45rem;
  }

  .slider span {
    width: 7.5rem;
    font-size: 1.6rem;
  }

  .left-label {
    text-align: right;
  }

  .right-label {
    text-align: left;
  }

  .etc {
    display: flex;
    margin-top: 1rem;
    gap: 2rem;
  }

  .etc * {
    width: 30rem;
    font-size: 1.6rem;
  }

  .result {
    margin-top: 2rem;
    font-size: 1.6rem;
  }
</style>
