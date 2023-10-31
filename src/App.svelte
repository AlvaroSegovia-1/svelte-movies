<script>
  import { fly } from "svelte/transition";
  import MovieItem from "./Movie/Item.svelte";
  import TailwindCss from "./lib/TailwindCSS.svelte";

  const APIKEY = "149683527c461b03e35657138b89dbf3";
  const BASEURL = `https://api.themoviedb.org/3`;
  const APISETTINGS = `?api_key=${APIKEY}&language=es-MX`;

  const movies = (async () => {
    const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`;
    const response = await fetch(URL);

    return await response.json();
  })();

  let likedMovies = [];

  function pulsaLike(event) {
    //console.log(event);
    const movie = event.detail;

    let index = likedMovies.findIndex((m) => m.id === movie.id);

    if (index >= 0) {
      likedMovies.splice(index, 1);
      //console.log(likedMovies);
    } else {
      likedMovies.push(movie);
    }

    likedMovies = likedMovies;
  }

  $: like = (id) => {
    let index = likedMovies.findIndex((m) => m.id === id);

    return index >= 0;
  };
</script>

<svelte:head>
  <title>Svelte-Movies</title>
</svelte:head>

<h1 class="text-5xl text-center font-bold m-6">Películas</h1>

<main class="mt-5 flex justify-center">
  <div class="w-8/12 flex flex-wrap justify-center panel">
    {#await movies}
      <div class="">
        <p>Cargando datos...</p>
      </div>
    {:then data}
      <!-- {@debug data} -->
      <!-- promise was fulfilled -->
      {#each data.results as movie}
        <div class="white m-2 rounded-lg overflow-y-auto h-96">
          <MovieItem
            like={like(movie.id)}
            id={movie.id}
            title={movie.title}
            overview={movie.overview}
            cover={movie.poster_path}
            on:onToggleLike={pulsaLike}
          />
        </div>
      {:else}
        <p>No hay movies</p>
      {/each}
    {/await}
  </div>
  <div class="w-3/12 panel">
    <h2 class="text-2xl text-center font-bold m-3">Peliculas favoritas</h2>
    <div class="row">
      {#if likedMovies.length > 0}
        <!-- content here -->
        {#each likedMovies as movie, i (movie.id)}
          <div class="m-2 rounded-lg" transition:fly={{ duration: 200, y: 20 }}>
            <MovieItem
              like={like(movie.id)}
              id={movie.id}
              title={movie.title}
              overview=""
              cover={movie.cover}
              on:onToggleLike={pulsaLike}
            />
          </div>
        {/each}
      {:else}
        <!-- else content here -->
        <div>
          <p>No hay peliculas favoritas</p>
        </div>
      {/if}
    </div>
  </div>
</main>

<style>
  .panel {
    height: 100vh;
    overflow: auto;
  }
</style>
