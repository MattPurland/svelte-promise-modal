<script>
  import CenterBox from "./centerbox.svelte";
  import { createEventDispatcher } from "svelte";
  import { fade, fly } from "svelte/transition";

  let show, wrapper;

  export let cssClass = "",
    transition = "fly",
    dangerMode;

  let trans,
    transOptions = {};

  if (transition == "fly") {
    trans = fly;
    transOptions = { y: 200 };
  } else if (transition == "fade") {
    trans = fade;
  }

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
    show = false;
  }

  // Move modal to the root of the body
  $: if (wrapper) {
    //document.body.appendChild(wrapper);
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
    class="modal-wrapper {cssClass}"
    in:fade
    out:fade={{ delay: 200 }}
    bind:this={wrapper}
    on:click={() => {
      handleClose();
    }}
  >
    <CenterBox>
      <div
        in:trans={transOptions}
        out:trans={transOptions}
        class="modal"
        on:click={(event) => {
          event.stopPropagation();
        }}
      >
        <slot />
      </div>
    </CenterBox>
  </div>
{/if}
