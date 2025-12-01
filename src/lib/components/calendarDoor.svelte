<script lang="ts">
	import Locker from "./icons/Locker.svelte";
	import MovieInfoModal from "./modals/MovieInfoModal.svelte";

    let { dayIndex, clickFunction } = $props();

    const today = new Date();
    const currentDay = today.getMonth() === 11 ? today.getDate() : 0;
    let dayUnlocked = $state(dayIndex <= currentDay);
</script>

<button
   
    onclick={clickFunction}
    class="w-20 h-20 md:w-24 md:h-24 flex items-center justify-center text-4xl text-white bg-[#9D2B24]">
    <div class="w-16 h-16 md:w-20 md:h-20 flex items-center justify-center border-[#6a2a21] border-2">
        {#if dayUnlocked}
            <span class="dayIndex font-bold">{dayIndex}</span>
        {:else}
            <Locker />
        {/if}
    </div>
</button>