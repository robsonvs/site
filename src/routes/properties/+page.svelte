<script lang="ts">
	import Title from "$lib/components/Title.svelte"
	import { collection, getDocs, doc, deleteDoc } from 'firebase/firestore'
    import { db } from '$lib/firebase/firebase'
	import { onMount } from "svelte"
	import { Button, Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell, Modal, Alert} from 'flowbite-svelte'
	import { InfoCircleSolid } from 'flowbite-svelte-icons'

	let properties: any[] = $state([])
	let modal = $state({value: false, id: '', name: ''})
	
	onMount(async () => {

		try {
			const query = await getDocs(collection(db, 'properties'));
			properties = query.docs.map(docs => ({ id: docs.id, ...docs.data() }));
		} catch (e) {
			console.log("PROBLEM TO CONNECT: " + e)
		}
	})

	const deleteItem = async (id: string) => {
		try{
			await deleteDoc(doc(db, 'properties', id));
			properties = properties.filter(prop => prop.id !== id);
		} catch (error) {
      		console.error(error);
    	}
  	}
</script>

<Title title="Properties" header="properties"/>
  
{#await properties}
	<div>Loading...</div>
{:then data}
<Table striped={true} hoverable={true}>
	<TableHead>
		<TableHeadCell>Image</TableHeadCell>
		<TableHeadCell>Name</TableHeadCell>
		<TableHeadCell>Description</TableHeadCell>
		<TableHeadCell><Button href="/properties/new" color="blue">Add</Button></TableHeadCell>
	</TableHead>
	{#if properties.length === 0}
		<TableBody tableBodyClass="divide-y" >
			<TableBodyRow>
				<TableBodyCell>
					<Alert border color="red">
						<InfoCircleSolid slot="icon" class="w-5 h-5" />
						<span class="font-medium">Alert: </span>
						No properties found.
					</Alert>
				</TableBodyCell>
			</TableBodyRow>
		</TableBody>	
	{:else}
		<TableBody tableBodyClass="divide-y">
			{#each properties as prop, i}
				<TableBodyRow>
					<TableBodyCell><img src={prop.image} alt={prop.name}/></TableBodyCell>
					<TableBodyCell>{prop.name}</TableBodyCell>
					<TableBodyCell>{prop.description}</TableBodyCell>
					<TableBodyCell>
						<Button href="/properties/{prop.id}" color="yellow">Edit</Button>
						<Button onclick={() => { modal.value = true; modal.id = prop.id; modal.name = prop.name; }} color="red">Delete</Button>
					</TableBodyCell>
				</TableBodyRow>				
			{/each}
		</TableBody>
		{/if}
	</Table>
{:catch error}
	<div>{error.message}</div>
{/await}

<Modal title="Delete" bind:open={modal.value} autoclose>
	<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">Are you sure you want to delete '{modal.name}' item?</p>
	<svelte:fragment slot="footer">
	<Button onclick={() => deleteItem(modal.id)}>Yes</Button>
	<Button color="alternative">No</Button>
	</svelte:fragment>
</Modal>

<style>
	img {
		height: 30px;
		width: 50%;
		object-fit: cover;
	}
</style>