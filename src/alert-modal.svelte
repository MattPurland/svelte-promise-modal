<script>
  import Modal from "./modal.svelte";

  export let cssClass,
    transition,
    dangerMode = false,
    maxWidth;

  let mainmodal;

  let _resolve;

  let alertContent = "";

  const confirm = () => {
    if (_resolve) {
      _resolve();
    } else {
      throw new Error("confirm was called before modal was asked");
    }
    mainmodal.close();
  };

  export const alert = async (text) => {
    alertContent = text;
    mainmodal.open();
    return new Promise((resolve, reject) => {
      _resolve = resolve;
    });
  };
</script>

<Modal
  bind:this={mainmodal}
  on:close={confirm}
  {transition}
  {cssClass}
  {dangerMode}
  {maxWidth}
>
  <slot name="header" {confirm} />
  {@html alertContent}
  <slot name="footer" {confirm} />
</Modal>
