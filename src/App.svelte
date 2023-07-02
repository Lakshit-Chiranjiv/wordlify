<script>
  let searchQuery = "";
  let searchQueryShow = "";
  let searchResult = "";
  let loading = false;

  let cache = {};

  function searchDictionary() {
    loading = true;
    searchResult = "";

    if (cache[searchQuery.toLowerCase()]) {
      searchQueryShow = searchQuery;
      searchResult = cache[searchQuery];
      loading = false;
      return;
    }

    searchQueryShow = searchQuery;
    fetch(`https://api.api-ninjas.com/v1/dictionary?word=${searchQuery}`, {
      headers: {
        'X-Api-Key': import.meta.env.VITE_API_NINJA_APIKEY
      }
    })
      .then((res) => res.json())
      .then((data) => {
        if(data.valid){
          searchResult = data.definition
          cache[searchQuery.toLowerCase()] = data.definition;
        }
        else{
          searchResult = "No result found";
          cache[searchQuery.toLowerCase()] = "No result found";
        }
        loading = false;
      })
      .catch((err) => {
        searchResult = "No result found";
        loading = false;
      });
  }

  document.addEventListener("keydown", function (event) {
    if (event.key === "Enter") {
      searchDictionary();
    }
  });


</script>

<main>
  <div class="logodiv">
    <img src="wordlify-logo.png" alt="logo">
  </div>
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
  :root{
    color: rgba(255, 255, 255, 0.87);
    background-color: #242424;
  }
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
    padding: 10px;
    box-sizing: border-box;
  }
  main .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .logodiv{
    display: flex;
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
    text-align: center;
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
    border: 1px solid transparent;
    background-color: transparent;;
    min-width: 200px;
    font-family: 'Poppins', sans-serif;
    outline: none;
    border-bottom: 1px solid #fff;
    caret-color: #fff;
    color: #fff;
  }

  button {
    padding: 10px 20px;
    background: linear-gradient(to right, #ff416c, #ff4b2b);
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background: linear-gradient(to right, #ff4b2b, #ff416c);
    transform: scale(1.05);
    transition: all 0.2s ease-in-out;
  }

  button:active {
    transform: scale(0.95);
  }

  .result-container {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 20px;
    background-color: #7e7e7e;
    width: 90%;
    text-align: center;
  }

  h2 {
    font-size: 18px;
    margin-bottom: 10px;
  }

  p {
    font-size: 14px;
  }

  @media (max-width: 496px) {
    img{
      width: 300px;
    }
    h1 {
      font-size: 20px;
    }

    .search-container{
      flex-direction: column;
      gap: 10px;
    }
    button{
      width: 100%;
    }
  }

</style>
