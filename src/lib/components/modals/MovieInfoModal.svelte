<script lang="ts">
	import Close from "../icons/Close.svelte";

  let {info, closeModalFunction, movieDay} = $props();

   const today = new Date();
    const currentDay = today.getMonth() === 11 ? today.getDate() : 0;
    let dayUnlocked = $state(movieDay <= currentDay);
</script>

<div onclick={closeModalFunction} class="fixed inset-0 bg-[#000000b9] flex items-center justify-center z-50">
    <div class="max-w-[800px] max-h-[80vh] flex justify-center flex-col gap-5 overflow-y-auto p-5 rounded-lg shadow-lg bg-[#9D2B24]">
        <div class="flex justify-between items-center gap-5">
            <h1 class="text-[#f2d8a9] text-2xl">Filme de {movieDay} de dezembro</h1>
            <button class="cursor-pointer" onclick={closeModalFunction}><Close /></button>
        </div>

        {#if !dayUnlocked}
            <span class="text-[#f2d8a9] text-christmas">Surpresa 🎁 ! Volta no dia {movieDay} de dezembro para saberes o filme 🎅</span>
        {:else}
            <div class="flex flex-col text-[#f2d8a9]">
                <span class="text-4xl ">{info.title ?? "-"}</span>
                <span class="text-sm">{info.whereToWatch} {info.year}</span>
                <span></span>
            </div>
        <img class="w-full max-h-[50vh]" src={info.posterUrl} alt="">
       {/if}


    </div>
</div>
