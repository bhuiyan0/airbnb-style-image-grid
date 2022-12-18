<script lang="ts">
	import CloseIcon from '$lib/Icon/CloseIcon.svelte';
	import Carousel from './Carousel.svelte';

	export let images:any =[];
	export let imageShowingIndex=0;
	export let initialPageIndex:number;
	export let showCarousel;	

	$: console.info('initialPageIndex',initialPageIndex)
</script>


<div class="h-screen w-full bg-black z-[999] fixed top-0 left-0 ">
	<div class="carosel-header h-16 sticky top-0 w-full bg-black flex items-center justify-between px-8">
		<button on:click={()=>showCarousel=false} class="flex items-center gap-1 text-white hover:bg-gray-500 px-3 py-1.5 rounded-lg">
			<CloseIcon/>
			<span class=" font-medium">Close</span>
		</button>

		<div class="text-white font-medium">
			{imageShowingIndex+1} / {images.length}
		</div>
		<div></div>
	</div>

	<div class="h-[calc(100%-4rem)] relative w-full flex flex-col items-center justify-center">
		<Carousel {images} bind:currentPageIndex={imageShowingIndex} {initialPageIndex}/>
	</div>
</div>