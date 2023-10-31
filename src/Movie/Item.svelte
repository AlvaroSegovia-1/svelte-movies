<script>
  import { createEventDispatcher } from "svelte";

  export let id;
  export let title;
  export let overview;
  export let cover;
  export let like;

  $: coverURL = `https://image.tmdb.org/t/p/w185_and_h278_bestv2${cover}`;

  let dispatch = createEventDispatcher();

  function toggleLike() {
    dispatch("onToggleLike", { id, title, overview, cover });
  }
</script>

<div
  class="w-52 mx-1 p-1 bg-white border-solid border-2 border-black rounded-xl borde"
>
  <div class="flex justify-center">
    <img
      src={coverURL}
      class=" w-full mt-1 object-cover rounded-xl"
      alt="..."
    />
  </div>

  <div class=" p-2">
    <div class="flex">
      <h5 class="w-3/5 font-bold text-xs">
        {title}
      </h5>
      <button
        class="w-2/5 h-7 {like
          ? 'text-white text-xs/[10px] bg-green-600  px-2 py-1 rounded-md hover:bg-purple-800'
          : 'text-white text-xs bg-purple-600  px-2 py-1 rounded-md hover:bg-purple-800'}"
        on:click={toggleLike}
        >{like ? "Ya no me gusta" : "Me gusta"}
      </button>
    </div>

    <p class=" text-xs text-gray-600">
      {overview}
    </p>
    <!-- <p class={like ? (overview = " ") : overview} /> -->
    <!-- <p class={like ? (cover = false) : cover} /> -->

    <!-- <div class="flex justify-center my-4">
      <button
        class={like
          ? "text-white text-xs bg-green-600  px-3 py-1 rounded-md hover:bg-purple-800"
          : "text-white text-xs bg-purple-600  px-3 py-1 rounded-md hover:bg-purple-800"}
        on:click={toggleLike}>{like ? "Ya no me gusta" : "Me gusta"}</button
      >
    </div> -->
  </div>
</div>

<style>
</style>
