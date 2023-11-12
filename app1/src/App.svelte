<script>
  import { onMount } from "svelte";
  import hobbyStore from "./hobby-store.js";

  let hobbies = [];
  let hobbyInput;
  let isLoading = false;

  onMount(() => {
    isLoading = true;
    fetch(
      "https://svelte-course-d045b-default-rtdb.europe-west1.firebasedatabase.app/hobbies.json"
    )
      .then((res) => {
        if (!res.ok) {
          throw new Error("Failed");
        }
        return res.json();
      })
      .then((data) => {
        isLoading = false;
        hobbyStore.setHobbies(Object.values(data));
        let keys = Object.keys(data);
        console.log(keys);
      })
      .catch((err) => {
        isLoading = false;
        console.log(err);
      });
  });

  function addHobby() {
    hobbyStore.addHobby(hobbyInput.value);

    isLoading = true;

    fetch(
      "https://svelte-course-d045b-default-rtdb.europe-west1.firebasedatabase.app/hobbies.json",
      {
        method: "POST",
        body: JSON.stringify(hobbyInput.value),
        headers: {
          "Content-Type": "application/json",
        },
      }
    )
      .then((res) => {
        isLoading = false;
        if (!res.ok) {
          throw new Error("Failed");
        }
        alert("Saved data");
      })
      .catch((err) => {
        isLoading = false;
        console.log(err);
      });
  }
</script>

<label for="hobby">Hobby</label>
<input type="text" id="hobby" bind:this={hobbyInput} />
<button on:click={addHobby}>Add Hobby</button>

{#if isLoading}
  <p>Loading...</p>
{:else}
  <ul>
    {#each $hobbyStore as hobby}
      <li>{hobby}</li>
    {/each}
  </ul>
{/if}
