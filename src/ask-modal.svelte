<script>
  import Modal from "./modal.svelte";
  import { writable } from "svelte/store";

  export let cssClass,
      transition,
      dangerMode = false;

  let mainmodal;

  let _resolve;
  let _reject;

  let datastore = writable({});

  const confirm = () => {
      if (_resolve) {
          _resolve($datastore);
      } else {
          throw new Error("confirm was called before modal was asked");
      }
      mainmodal.close();
  };

  const cancel = () => {
      if (_reject) {
          _reject();
      } else {
          throw new Error("cancel was called before modal was asked");
      }
      mainmodal.close();
  };

  export const ask = async (defaultValues) => {
      datastore.set(defaultValues || {});
      mainmodal.open();
      return new Promise((resolve, reject) => {
          _resolve = resolve;
          _reject = reject;
      });
  };
</script>

<Modal bind:this={mainmodal} on:close={cancel} {transition} {cssClass} {dangerMode}>
  <slot name="header" {confirm} {cancel} />
  <slot {datastore} {confirm} {cancel} />
  <slot name="footer" {confirm} {cancel} />
</Modal>
