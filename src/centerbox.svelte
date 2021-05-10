<script>
  import { createEventDispatcher } from "svelte";

  export let dangerMode, show;

  let dispatch = createEventDispatcher(),
    centerBox;

  function handleClose(event) {
    // If dangerMode, do nothing
    if (dangerMode) return;

    const eventTarget =
      event.path && event.path.length > 0 ? event.path[0] : event.target;

    // Event target isn't the centerbox, do nothing
    if (eventTarget !== centerBox) return;

    // Otherwise close
    dispatch("close");
    show = false;
  }
</script>

<div
  {...$$props}
  class="center-wrapper {$$props.class || ''}"
  bind:this={centerBox}
  on:click={handleClose}
>
  <slot />
</div>
