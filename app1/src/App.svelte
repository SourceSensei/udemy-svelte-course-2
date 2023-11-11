<script>
  import CustomInput from "./CustomInput.svelte";
  import Toggle from "./Toggle.svelte";

  let val = "Sensei";
  let price = 0;
  let selectedOption = 1;

  $: console.log(val);
  $: console.log(selectedOption);
  $: console.log(price);

  function setValue(event) {
    val = event.target.value;
  }
</script>

<h1>Bindings & Forms</h1>

<!-- <input type="text" value={val} on:input={setValue}> -->
<!-- <input type="text" bind:value={val}> -->
<CustomInput bind:val />

<Toggle bind:chosenOption={selectedOption} />

<input
  type="number"
  bind:value={price}
/>
