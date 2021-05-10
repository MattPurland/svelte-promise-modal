<script>
  import CenterBox from "./centerbox.svelte";
  import { createEventDispatcher } from "svelte";
  import { fade, fly } from "svelte/transition";

  let show;

  export let maxwidth;
  export let dangerMode;

  export const open = () => {
    show = true;
  };

  export const close = () => {
    show = false;
  };

  let dispatch = createEventDispatcher();

  function handleClose() {
    if (dangerMode) return;

    dispatch("close");
    close();
  }
</script>

<svelte:window
  on:keydown={(event) => {
    if (event.which == 27 && show == true) {
      handleClose();
    }
  }}
/>

{#if show}
  <div
    class="modal-wrapper"
    in:fade
    out:fade={{ delay: 200 }}
    on:click={handleClose}
  >
    <CenterBox style="overflow-y:scroll">
      <div
        in:fly={{ y: 200 }}
        out:fly={{ y: 200 }}
        class="modal"
        style={`max-width: ${maxwidth}px`}
        on:click={(event) => {
          event.stopPropagation();
        }}
      >
        <slot />
      </div>
    </CenterBox>
  </div>
{/if}

<style>
  .modal-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.5);
    z-index: 99;
  }
  .modal {
    background: white;
    box-sizing: border-box;
    padding: 40px;
    width: 90vw;
    border-radius: 5px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
    z-index: 100;
  }
</style>
