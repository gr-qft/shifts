<script lang="ts">
  import type { IShift } from "../types/shifts.type";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let shifts: IShift[] = [];

  let selectedShift: IShift;

  function addShift() {
    if (!selectedShift) {
      return;
    }
    dispatch("addShift", selectedShift);
  }
</script>

<div>
  <h1>Add shift</h1>

  <select bind:value={selectedShift}>
    {#each shifts as shift}
      <option value={shift}> {shift.start} - {shift.end} </option>
    {/each}
  </select>

  <button on:click={addShift}> Add </button>
</div>
