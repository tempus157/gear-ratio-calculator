<script lang="ts">
  const gears = [3, 4, 5, 6, 7, 8, 9, 10] as const;

  let first = 3;
  let last = 1;
  let shape = 1;
  let count = 6;
  let result = "I'm ready to calculate!";

  $: actualShape = shape * count;

  function displayCalculated() {
    const magicValue = findMagicValue();
    if (magicValue === null) {
      alert("Shape is wrong");
      return;
    }

    result = "";
    for (let ratio of calculate(magicValue)) {
      result += `${ratio.toFixed(2)}<br />`;
    }
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
    <input type="range" min="0.48" max="6" step="0.01" bind:value={first} />
    <span>{first.toFixed(2)}</span>
  </div>

  <div>
    <input type="range" min="0.48" max="6" step="0.01" bind:value={last} />
    <span>{last.toFixed(2)}</span>
  </div>

  <div>
    <input type="range" min="0" max="5" step="0.01" bind:value={shape} />
    <span>{shape.toFixed(2)}</span>
  </div>

  <select bind:value={count}>
    {#each gears as gear}
      <option value={gear}>{gear} Speed</option>
    {/each}
  </select>
  <button on:click={displayCalculated}>Calculate</button>

  <h3>Result</h3>
  <div>{@html result}</div>
</div>
