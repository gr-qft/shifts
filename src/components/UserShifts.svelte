<script lang="ts">
  import type { IShift } from "../types/shifts.type";

  export let shifts: IShift[] = [];
</script>

<div>
  <h1>User Shifts</h1>
  <ul>
    {#each shifts as shift}
      <li>
        {shift.start} - {shift.end}
      </li>
    {/each}
  </ul>
</div>
