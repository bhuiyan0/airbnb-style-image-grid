<script lang="ts">
	import {fly} from 'svelte/transition';
	import { onDestroy, onMount } from 'svelte';
	import {Button} from 'flowbite-svelte'

	import LeftArrowIcon from '$lib/Icon/LeftArrowIcon.svelte';
	import CarouselModal from './CarouselModal.svelte';
	
	export let showModal:boolean;
	export let focus_image_url:string;
	export let initialPageIndex:number;
	export let images:any[] = [];

	const big = [0,3,5,9,12,15,18,21,24];

	onMount(()=>{
		if(focus_image_url){
			const el = document.getElementById(focus_image_url);
			if (!el) return;
			el.scrollIntoView({
				behavior: 'smooth',
				block:'center'
			});
		}
	})

	onDestroy(()=>{
		focus_image_url='';
	})

	let showCarousel=false;
	let imageShowingIndex=0;

	const imageClickHandler = (index:number)=>{
		initialPageIndex=index;
		imageShowingIndex=index;
		showCarousel=true;
	}
</script>
 

<div transition:fly={{duration:500,y:1000}} class="fixed h-screen top-0 right-0 bottom-0 left-0 z-[999] bg-white">
	<div class="h-16 sticky top-0 w-full flex items-center px-3">
		<Button color="alternative" btnClass="hover:bg-gray-200 hover:border-gray-500 border-white rounded-full p-1.5" pill={true} outline={true} class="!p-2" size="xl" on:click={()=>{showModal=false}}>
			<LeftArrowIcon/>
		</Button>
	</div>

	<div class="image-grid-container">

		<div class="h-full overflow-auto">
			<div class="h-auto w-full max-w-2xl p-4 mx-auto pb-10">		
				<div class="container  grid gap-2 h-full grid-cols-2 grid-flow-row ">
					{#each images as item,index}		
						<button id="{item.image_url}" on:click={()=>imageClickHandler(index)} class="image-wrapper {big.includes(index) ? 'big' : 'vertical'} w-full  cursor-pointer  transition-opacity duration-200">
							<img class="object-cover w-full h-full aspect-square" src="{item?.image_url}" alt="{item?.tags}">
						</button>
					{/each}
				</div>
			</div>
		</div>
	</div>

</div>

{#if showCarousel}
	<CarouselModal initialPageIndex={initialPageIndex} bind:showCarousel {images} bind:imageShowingIndex/>
{/if}


<style lang="postcss">
	.image-grid-container{
		@apply absolute top-16 bottom-0 h-auto w-full;
	}

	.container {
		@apply grid grid-flow-dense grid-cols-2 ;
	}

	.container img {
		@apply w-full h-full max-h-[400px] object-cover;
	}

	.vertical {
		@apply max-h-[200px];
	}

	.big {
		@apply col-span-2 row-span-2;
	}

	.image-wrapper{
		@apply relative hover:after:opacity-10;
	}
	.image-wrapper:after{
		@apply absolute content-[''] bg-black left-0 opacity-0 top-0 w-full h-full z-10 
		transition-opacity ease-in-out duration-200;
	}
</style>