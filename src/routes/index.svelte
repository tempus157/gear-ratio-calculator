<script lang="ts">
  const gears = [3, 4, 5, 6, 7, 8, 9, 10] as const;

  let first = 3;
  let last = 3;
  let decrease = 1;
  let count = 6;
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
  <button type="button">Calculate</button>

  <h3>Result</h3>
  <div />
</div>
