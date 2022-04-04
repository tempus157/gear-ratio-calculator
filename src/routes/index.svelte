<script lang="ts">
  const gearSpeeds = [3, 4, 5, 6, 7, 8, 9, 10] as const;
  const minGearRatio = 0.48;
  const maxGearRatio = 6;

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
    <label>
      <span>First</span>
      <input type="range" min="0.48" max="6" step="0.01" bind:value={first} />
      <span>{first.toFixed(2)}</span>
    </label>
  </div>

  <div>
    <label>
      <span>Last</span>
      <input type="range" min="0.48" max="6" step="0.01" bind:value={last} />
      <span>{last.toFixed(2)}</span>
    </label>
  </div>

  <div>
    <label>
      <span>Shorter</span>
      <input type="range" min="0" max="2" step="0.01" bind:value={shape} />
      <span>Longer</span>
    </label>
  </div>

  <div>
    <select bind:value={count}>
      {#each gearSpeeds as gear}
        <option value={gear}>{gear} Speed</option>
      {/each}
    </select>
    <button on:click={displayCalculated}>Calculate</button>
  </div>

  <div>
    {#each result as value, index}
      <div>
        <span>{index + 1}:</span>
        <span>{value}</span>
      </div>
    {/each}
  </div>
</div>
