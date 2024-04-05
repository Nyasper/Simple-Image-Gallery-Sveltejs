<script lang="ts">
  import { fade, fly,  } from "svelte/transition"
  //Data to Fetch
  interface imagesInterface {
    id: string;
    alt_description: string;
    urls: { regular: string };
  }
  let images:imagesInterface[] = [];
  const fetchImages = async(searchParam:string):Promise<imagesInterface[]> => {
    const res = await fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchParam}&client_id=PEFdmX_fy3WdLqss1uIJpNbhUf67bGRo7DuziCIjelE`
  )
  const data = await res.json();
  return data.results
}
  //Search Bar
  let searchInputValue = "cats"; 
  const handleSearch = async () => {
    images = await fetchImages(searchInputValue||"cats");
    searchInputValue = "";
  }

</script>

<div class="container">
  <div class="header">
    <h1>Image Gallery</h1>
    <div class="input-container">
      <input type="text" class="input" bind:value={searchInputValue}>
      <button class="button" on:click={handleSearch}>
        Search
      </button>
    </div>
  </div>
  <div class="photos">
    {#each images as image, i (image.id)}
      <img
        src={image.urls.regular}
        alt={image.alt_description}
        class="image"
        in:fly={{y:150, duration: 1500, delay: i*200}}
        out:fade = {{duration: 200}}
      />
    {/each}
  </div>
</div>

<style>

  .image {
    /* width: 400px; */
    height: 250px;
    margin: 5px;
  }
  .photos {
    display: flex;
    flex-wrap: wrap;
  }
  .container {
    width: 1230px;
    margin: 0 auto;
  }
  .header {
    text-align: center;
    font-size: 20px;
  }
  .input {
    padding: 10px;
    width: 400px;
    border-radius: 10px;
    outline: none;
    border: 1px solid gray;
    font-size: 20px;
  }
  .button {
    padding: 10px;
    font-size: 20px;
    background-color: rgb(0, 255, 200);
    border-radius: 10px;
    border: none;
    color: white;
    cursor: pointer;
  }
  .input-container {
    margin-bottom: 40px;
  }
</style>