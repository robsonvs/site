<script lang="ts">
	import Title from "$lib/components/Title.svelte";
	import { Heading, P, Button, Span, Card, Rating, Badge  } from 'flowbite-svelte';
  	import { ArrowRightOutline } from 'flowbite-svelte-icons';
	import { collection, getDocs, doc } from 'firebase/firestore'
    import { db } from '$lib/firebase/firebase'
	import { onMount } from "svelte"

	let properties:any = []
	
	onMount(async () => {

		try {
			const query = await getDocs(collection(db, 'properties'));
			properties = query.docs.map(docs => ({ id: docs.id, ...docs.data() }));
		} catch (e) {
			console.log("PROBLEM TO CONNECT: " + e)
		}
	})

	let form = {
		firstName: String(),
		lastName: String(),
		birthday: Date()
	}
</script>

<div class="hero">
	<Heading tag="h1" 
			class="mb-4"
			customSize="text-4xl font-extrabold  md:text-5xl lg:text-6xl">
		<Span gradient>Better Data</Span>
	</Heading>
	
	<P class="mb-6 text-lg lg:text-xl sm:px-16 xl:px-48 dark:text-gray-400">
		Here at Flowbite we focus on markets where technology, innovation, and capital can unlock long-term value and drive economic growth.
	</P>
	
	<Button href="/">
	  Learn more
	  <ArrowRightOutline class="w-6 h-6 ms-2" />
	</Button>
</div>

<Title titleAndHeader="Home" />

 
{#await properties}
	<div>Loading...</div>
{:then data} 
	<div class="grid sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 p-4">
		{#each properties as prop, i}
			<div class="space-4 min-w-fit max-w-full transition duration-700 ease-in-out transform hover:scale-105">
				<Card padding="none" class="rounded-lg shadow-2xl">
					<a href={prop.name}>
						<img class="rounded-t-lg" src={prop.image} alt={prop.image} />
					</a>
					<div class="px-5 pb-5">
						<h5 class="text-xl font-semibold tracking-tight text-gray-900 dark:text-white">{prop.name}</h5>
						<p class="mb-3 font-normal text-gray-700 dark:text-gray-400 leading-tight card">{prop.description}</p>
						
						<Rating rating={4} size={24} class="mt-2.5 mb-5">
							<Badge slot="text" class="ms-3">4</Badge>
						</Rating>
						<div class="flex justify-between items-center">
							<span class="text-3xl font-bold text-gray-900 dark:text-white">$599</span>
							<Button href="/">Buy now</Button>
						</div>
					</div>
				</Card>
			</div>	
		{/each}
	</div>
{:catch error}
	<div>{error.message}</div>
{/await}

<form>
	<input id="first" 
		type="text" 
		name="first" 
		bind:value={form.firstName} 
		required
		placeholder="First Name:"/>
	
	<input id="last" 
		type="text" 
		name="last" 
		required
		bind:value={form.lastName} 
		placeholder="Last Name:"/>
	
	<input id="bithday" type="date" name="birthday" bind:value={form.birthday} required placeholder="Birthday"/>

	<button>save</button>
</form>

<style>
	.hero{
		margin: 0 auto;
		padding: 2rem;
		background-image: repeating-conic-gradient(silver 10%, black 20%); /* Used if the image is unavailable */
		height: 300px; /* You must set a specified height */
		width: 100%;
		background-image: url(''), url('https://www.rpnation.com/gallery/12-background-gif-210-1.26848/full'); /* The image used */
		background-repeat: no-repeat; /* Do not repeat the image */
		background-position: center; /* Center the image */
		text-align: center;
		padding: 10px;
		background-size: cover; /* Resize the background image to cover the entire container */
		position: relative;
	}

	input {
		padding: 0.5rem;
  		border: double 2px transparent;
  		border-radius: 30px;
  		background-image: linear-gradient(to top right, white, white), 
  							linear-gradient(to top left, #743ad5, #d53a9d);
  		background-origin: border-box;
  		background-clip: padding-box, border-box;		
	}

	img {
		height: 200px;
		width: 100%;
		object-fit: cover;
	}

	.card {
		height: 100px;
		width: 100%;
		object-fit: cover;
	}
</style>