<script lang="ts">
	import LeftArrowIcon from '$lib/Icon/LeftArrowIcon.svelte';
	import { onMount } from 'svelte';

	export let images:any;
	export let currentPageIndex:number;
	export let initialPageIndex:number;
	export let openImageGridModal:any;

	let Carousel:any;
	
	onMount(async () => {
		const module = await import('svelte-carousel');
		Carousel = module.default;
	});

</script>

<svelte:component
	let:showPrevPage
  let:showNextPage
	this={Carousel} {...{dots:false,arrows:true,initialPageIndex:initialPageIndex}}
	on:pageChange={event=> {currentPageIndex = event.detail} }>

  {#each images as image}
		<button on:click={()=>openImageGridModal(image.image_url)} class="image-wrapper">
			<img  src="{image.image_url}" alt="hello">
		</button>
	{/each}

	<button on:click={showPrevPage} slot="prev" class="arrows left-5">
		<LeftArrowIcon />
	</button>
	<button on:click={showNextPage} slot="next" class="arrows rotate-180 right-5">
		<LeftArrowIcon />
	</button>
</svelte:component>


<style>
	.arrows{
		@apply fixed top-1/2 -translate-y-1/2 bg-black z-[99] border-2 border-white text-white 
		hidden md:flex cursor-pointer items-center justify-center rounded-full w-12 hover:bg-gray-700 aspect-square px-2 py-1;
	}

	.image-wrapper{
		@apply w-full md:w-auto max-w-xl flex items-center justify-center;
	}

	.image-wrapper img{
		@apply w-full min-h-[300px] md:w-auto aspect-[3/2] md:aspect-auto max-h-[500px] md:max-h-[auto] object-cover;
	}
</style>