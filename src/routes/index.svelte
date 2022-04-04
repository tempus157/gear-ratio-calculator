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
  $: first, last, validateLast();

  function validateLast() {
    if (first < last) {
      last = first;
    }
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
      <span class="left-label">Shorter</span>
      <input type="range" min="0" max="2" step="0.01" bind:value={shape} />
      <span class="right-label">Longer</span>
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

<style lang="scss">
  .slider {
    display: flex;

    input {
      width: 100%;
    }

    span {
      width: 25%;
    }
  }

  .left-label {
    margin-right: 2%;
    text-align: right;
  }

  .right-label {
    margin-left: 2%;
    text-align: left;
  }

  .etc {
    display: flex;
    justify-content: space-around;

    * {
      width: 100%;
      margin: 2%;
    }
  }

  .result {
    margin: 0 2% 0 2%;
  }
</style>
