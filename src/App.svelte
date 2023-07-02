<script>
  let searchQuery = "";
  let searchQueryShow = "";
  let searchResult = "";
  let loading = false;

  function searchDictionary() {
    loading = true;
    searchResult = "";
    fetch(`https://api.api-ninjas.com/v1/dictionary?word=${searchQuery}`, {
      headers: {
        'X-Api-Key': import.meta.env.VITE_API_NINJA_APIKEY
      }
    })
      .then((res) => res.json())
      .then((data) => {
        if(data.valid){
          searchQueryShow = searchQuery;
          searchResult = data.definition
        }
        else{
          searchResult = "No result found";
        }
        loading = false;
      })
      .catch((err) => {
        searchResult = "No result found";
        loading = false;
      });
  }
</script>

<main>
  <img src="wordlify-logo.png" alt="logo">
  <h1>Dictionary App</h1>
  <div class="container">
    <div class="search-container">
      <input type="text" bind:value="{searchQuery}" placeholder="Enter a word">
      <button on:click="{searchDictionary}">Search</button>
    </div>
    {#if searchResult}
      <div class="result-container">
        <h2>{searchQueryShow}</h2>
        <p>{searchResult}</p>
      </div>
    {/if}
    {#if loading}
      <p>Loading...</p>
    {/if}
  </div>
</main>

<style>
  @font-face {
      font-family: 'Poppins';
      font-style: normal;
      font-weight: 400;
      src: url(https://fonts.gstatic.com/s/poppins/v20/pxiEyp8kv8JHgFVrJJfecg.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
  }

  @font-face {
      font-family: 'Poppins';
      font-style: normal;
      font-weight: 600;
      src: url(https://fonts.gstatic.com/s/poppins/v20/pxiByp8kv8JHgFVrLEj6Z1xlFQ.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
  }
  
  main{
    min-height: 100vh;
    font-family: 'Poppins', sans-serif;
  }
  main .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  img{
    width: 400px;
    margin-bottom: 20px;
  }

  h1 {
    font-size: 24px;
    margin-bottom: 20px;
  }

  .search-container {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    margin-bottom: 20px;
  }

  input {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    min-width: 200px;
  }

  button {
    padding: 10px 20px;
    background: linear-gradient(to right, #ff416c, #ff4b2b);
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .result-container {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 20px;
    background-color: #7e7e7e;
    width: 300px;
    text-align: center;
  }

  h2 {
    font-size: 18px;
    margin-bottom: 10px;
  }

  p {
    font-size: 14px;
  }
</style>
