<script>
  import { onMount } from 'svelte';
  import OverlayScrollbars from 'overlayscrollbars';

  let centerBox;

  onMount(() => {
      // OverlayScrollbars(centerBox, {
      //     scrollbars: {
      //         autoHide: "scroll"
      //     }
      // });
  });
</script>

<div {...$$props} class="center-wrapper {$$props.class || ''}" bind:this={centerBox}>
  <slot />
</div>
