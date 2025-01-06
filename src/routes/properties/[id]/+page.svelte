<script lang="ts">
	import Title from "$lib/components/Title.svelte";
    import { Input, Label, Helper, Button } from 'flowbite-svelte';
    import { collection, doc, addDoc, updateDoc, getDoc } from 'firebase/firestore'
    import { db } from '$lib/firebase/firebase'
    import { goto } from "$app/navigation"
    import { page } from '$app/state';
	import type { PageData } from './$types';

	let { data }: { data: PageData } = $props();

    let id = page.params.id;
    
    let property = $state({
                name: String(),
                description: String(),
                image: String()
    })

    const load = async () => {
        if(id !== "new" || id === undefined) {

            const docRef = doc(db, 'properties', id)
            const docSnap = await getDoc(docRef)
            data = { id: docSnap.id, ...docSnap.data() }
            property = {
                name: data.name,
                description: data.description,
                image: data.image
            }
        } 
    }
    load()

    const save = async () => {

        try {
            if (property.name.length === 0) 
                return { sucsses: false, message: 'Name cannot be empty.' }
            if(id === "new" || id === undefined) {
                await addDoc(collection(db, 'properties'), property)
                //return { sucsses: true, message: `The Property "${property.name}" was successfully created.` }
            } else {
                await updateDoc(doc(db, 'properties', id), property)
                //return { sucsses: true, message: `The Property "${property.name}" was successfully updated.` }
            }
            goto('/properties')
        } catch (error) {  
            return { sucsses: false, message:  `Erro: ${name} canoot be added!` }
        }
    }
</script>

<Title titleAndHeader={id === "new" || id === undefined ? "Add Property" : "Edit Property"} />

<form autocomplete="off" class="p-10">
    <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
            <Label for="name" class="mb-2">Name</Label>
            <Input id="name"
                    type="text" 
                    name="name" 
                    placeholder="Name" 
                    required 
                    bind:value={property.name} 
                    class="input"/>
            {#if property.name.length === 0}
                <Helper class="mt-2" color="red">
                    <span class="font-medium">Error:</span>
                    Name cannot be empty.
                </Helper>
            {:else if property.name.length < 3}
                <Helper class="mt-2" color="red">
                    <span class="font-medium">Error:</span>
                    Name should have a 3 characters or more.
                </Helper>
            {:else}
                <Helper class="mt-2" color="green">
                    <span class="font-medium">Well done!</span>
                    Some success message.
                </Helper>
            {/if}
        </div>
        <div>
            <Label for="description" class="mb-2">Description</Label>
            <Input type="text" name="description" placeholder="Description" required bind:value={property.description} class="fields"/>
            {#if property.description.length === 0}
                <Helper class="mt-2" color="red">
                    <span class="font-medium">Error:</span>
                    Description cannot be empty.
                </Helper>
            {:else if property.description.length < 3}
                <Helper class="mt-2" color="red">
                    <span class="font-medium">Error:</span>
                    Description should have a 3 characters or more.
                </Helper>
            {:else}
                <Helper class="mt-2" color="green">
                    <span class="font-medium">Well done!</span>
                    Some success message.
                </Helper>
            {/if}
        </div>
        <div>
            <Label for="image" class="mb-2">Image</Label>
            <Input type="text" name="image" placeholder="Image" required bind:value={property.image} class="fields"/>
            {#if property.image.length === 0}
                <Helper class="mt-2" color="red">
                    <span class="font-medium">Error:</span>
                    Image cannot be empty.
                </Helper>
            {:else if property.image.length < 3}
                <Helper class="mt-2" color="red">
                    <span class="font-medium">Error:</span>
                    Image should have a 3 characters or more.
                </Helper>
            {:else}
                <Helper class="mt-2" color="green">
                    <span class="font-medium">Well done!</span>
                    Some success message.
                </Helper>
            {/if}
        </div>
    </div>

    <div class="grid gap-6 mb-6 md:grid-cols-2">
        <div>
            <Button onclick={save} color="primary" size="sm" class="w-full">
                Save
            </Button>
        </div>
        <div>
            <Button onclick={() => history.back()} color="red" size="sm" class="w-full">
                Cansel
            </Button>
        </div>
      </div>
</form>

{JSON.stringify(property)}


<style>
	.input:invalid {
		padding: 0.5rem;
        border: double 2px transparent;
        border-color: #d53a9d;
        border-radius: 30px;
        background-image: linear-gradient(transparent, transparent)
  					        linear-gradient(to top left, #743ad5, #d53a9d);
        background-origin: border-box;
        background-clip: padding-box, border-box;	
	}
</style>