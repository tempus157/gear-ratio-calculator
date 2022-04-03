<script lang="ts">
  const gears = [3, 4, 5, 6, 7, 8, 9, 10] as const;

  let first = 3;
  let last = 1;
  let decrease = 1;
  let count = 6;
  let result = "I'm ready to calculate!";

  function display() {
    const actualDecrease = decrease * count;
    const shrink = findShrink(count, first, last, actualDecrease);

    if (shrink == undefined) {
      alert("Decrease is wrong");
      return;
    }

    result = "";
    for (let gear of calculate(count, first, actualDecrease, shrink)) {
      result += `${gear}<br />`;
    }
  }

  function findShrink(
    count: number,
    first: number,
    last: number,
    decrease: number
  ) {
    let min = 0;
    let max = 10;
    let shrink;

    while (min < max) {
      shrink = (min + max) / 2;
      const value = getLastGear(count, first, decrease, shrink);

      if (value > last) {
        max = shrink;
      } else if (value < last) {
        min = shrink;
      } else {
        return shrink;
      }
    }

    return undefined;
  }

  function getLastGear(
    count: number,
    first: number,
    decrease: number,
    shrink: number
  ) {
    let result;
    for (let value of calculate(count, first, decrease, shrink)) {
      result = value;
    }
    return round(result);
  }

  function* calculate(
    count: number,
    first: number,
    decrease: number,
    shrink: number
  ) {
    let result = first;
    yield result;

    for (let i = 1; i < count; i++) {
      const x = i / (count - 1);
      result -= result / (decrease * x + shrink);
      yield result;
    }
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
    <input type="range" min="0" max="5" step="0.01" bind:value={decrease} />
    <span>{decrease.toFixed(2)}</span>
  </div>

  <select bind:value={count}>
    {#each gears as gear}
      <option value={gear}>{gear} Speed</option>
    {/each}
  </select>
  <button type="button" on:click={display}>Calculate</button>

  <h3>Result</h3>
  <div>{first} {last} {decrease} {count}</div>
  <div>{@html result}</div>
</div>
