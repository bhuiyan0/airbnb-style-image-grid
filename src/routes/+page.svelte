<script lang="ts">
	import { Button } from 'flowbite-svelte'
	import data from '$lib/data/image_data.json';
	import GridIcon from '$lib/Icon/GridIcon.svelte'
	import ImageGridModal from '$lib/Components/ImageGridModal.svelte';
	import Carousel from '$lib/Components/Carousel.svelte';

	let property =  data[0];
	
  let showModal = false;
	let focus_image_url = '';
	let currentPageIndex=0;
	let initialPageIndex=0;


	const showAllHandler = (image_url?:string)=>{
		if(image_url && typeof image_url=='string'){
			focus_image_url=image_url;
		}
		showModal=true;
	} 

	
</script>

<div class="h-inherit w-full max-w-[1130px] md:p-4 mx-auto">
	<div class="block md:hidden h-auto relative">
		<Carousel {showAllHandler} bind:currentPageIndex {initialPageIndex} images={property.images} />

		<span class="page-number">
			{currentPageIndex+1} / {property.images.length}
		</span>
	</div>
	
	<h1 class="text-3xl font-medium py-3 md:px-2">Scenic A-frame retreat, hot tub, treehouse & more!</h1>

	<div class="relative hidden md:flex w-full h-auto gap-2 ">

		<div class="absolute bottom-5 right-5 z-10">
			<Button color="light" size="xs" on:click={()=>showAllHandler()}>
				<span class="pr-1"><GridIcon/> </span>
				<span>Show all photos</span>
			</Button>
		</div>


		<button on:click={()=>showAllHandler()} class="image-wrapper w-1/2 h-full">
			<img class=" object-cover aspect-square rounded-l-lg" src="{property?.images[0]?.image_url}" alt="">
		</button>

		<div class="images-container ">
			{#each property.images.filter((_,i)=>i<=5 && i>1) as item}	
				<button on:click={()=>showAllHandler(item.image_url)} class="image-wrapper ">
					<img class="object-cover aspect-square" src="{item?.image_url}" alt="{item?.tags}">
				</button>
			{/each}
		</div>
	</div>

	{#if showModal}
		<ImageGridModal bind:focus_image_url bind:showModal images={property.images}/>
	{/if}
</div>

<style>
	.images-container{
		@apply w-1/2 h-full grid grid-cols-2 gap-2;
	}

	.images-container .image-wrapper:nth-child(2) img{
		@apply rounded-tr-xl;
	}

	.images-container .image-wrapper:nth-child(4) img{
		@apply rounded-br-xl;
	}

	.image-wrapper{
		@apply cursor-pointer  transition-opacity duration-200 relative hover:after:opacity-10;
	}

	.image-wrapper:after{
		@apply absolute content-[''] bg-black left-0 opacity-0 top-0 w-full h-full z-10 
		transition-opacity ease-in-out duration-200;
	}

	.page-number{
		@apply absolute bottom-5 right-5 text-white px-2.5 bg-gray-800 rounded-md font-medium py-0.5 bg-opacity-50;
	}
</style>