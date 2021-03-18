<script lang="ts">
  import type { IShift } from "./types/shifts.type";
  import UserShifts from "./components/UserShifts.svelte";
  import AddShift from "./components/AddShift.svelte";

  let usershifts: IShift[] = [
    {
      start: "0000",
      end: "0010",
    },
    {
      start: "1200",
      end: "1300",
    },
  ];

  const availableCompanyShifts: IShift[] = [
    {
      start: "0000",
      end: "2359",
    },
    {
      start: "0600",
      end: "1800",
    },
    {
      start: "0010",
      end: "0020",
    },
    {
      start: "0600",
      end: "1200",
    },
    {
      start: "1800",
      end: "2359",
    },
    {
      start: "0000",
      end: "0600",
    },
    {
      start: "1200",
      end: "2359",
    },
    {
      start: "1200",
      end: "1800",
    },
  ];

  // Determine if anotherShift is in conflict with a given shift
  // We visualize each shift as an interval: ----------. There are four cases of
  // overlapping. In the drawing below, shift is top, anotherShift is bottom.
  // Note: the logic could possibly be simplified by combining the conditional clauses.
  // 1)        ----------
  //                ----------
  //
  // 2)        ----------
  //      ----------
  //
  // 3) In this case, anotherShift is contained in the given shift.
  //         ----------
  //           ------
  //
  // 4) In this case, anotherShift contains completely the given shift
  //             ------
  //           ----------
  // Please note for case 3 and 4, containment means possibly touching at the edges also
  function shiftsOverlap(shift: IShift, anotherShift: IShift) {
    return (
      (shift.end > anotherShift.start && shift.end < anotherShift.end) ||
      (shift.start > anotherShift.start && shift.start < anotherShift.end) ||
      (shift.start <= anotherShift.start && shift.end >= anotherShift.end) ||
      (shift.start >= anotherShift.start && shift.end <= anotherShift.end)
    );
  }

  function addShift(shift: IShift) {
    const hasConflict = usershifts.some((s) => shiftsOverlap(shift, s));
    if (!hasConflict) {
      usershifts = [...usershifts, shift];
    }
    return;
  }
</script>

<main>
  <UserShifts shifts={usershifts} />
  <AddShift
    shifts={availableCompanyShifts}
    on:addShift={(e) => addShift(e.detail)}
  />
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
