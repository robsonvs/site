<script lang="ts">
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger, Dropdown, DropdownItem, DropdownDivider, DarkMode } from 'flowbite-svelte';
    import { ChevronDownOutline } from 'flowbite-svelte-icons';
    import { page } from '$app/state';
    import { onMount } from "svelte";
    
    import { collection, getDocs, doc, addDoc, updateDoc, deleteDoc } from 'firebase/firestore'
    import {db} from '$lib/firebase/firebase'

    let menu:any = $state()
    onMount(async () => {

		try {
			const menuQuery = await getDocs(collection(db, 'Menu'));
			menu = menuQuery.docs.map(doc => ({ id: doc.id, ...doc.data() }));
		} catch (e) {
			console.log("PROBLEM TO CONNECT: " + e)
		}
	})

    let activeUrl = $derived(page.url.pathname)

    const company = {name: 'NosCorp', logo: 'https://w7.pngwing.com/pngs/985/67/png-transparent-blue-logo-circle-logo-symbol-font-templates-blue-angle-text-thumbnail.png'}

    let greeting = $state("")
    
    $effect(() => {
        const now = new Date();
        const hours = now.getHours()

        if (hours < 12) {
            greeting = "Good morning!"
        } else if (hours < 18) {
            greeting = "Good afternoon!"
        } else {
            greeting = "Good evening!"
        }
    })
</script>

{#snippet navItems(name: string, href: any, sub: [])}
    {#if href}
        <NavLi {href}>{name}</NavLi>
    {:else} 
        <NavLi class="cursor-pointer">
            {name}<ChevronDownOutline class="w-6 h-6 ms-2 text-primary-800 dark:text-white inline" />
        </NavLi>
        <Dropdown {activeUrl} class="w-44 z-20">
            {#each sub as {href, name}, i}
                <DropdownItem {href}>{name}</DropdownItem>
            {/each}
        </Dropdown>
    {/if}
{/snippet}

<Navbar>
    <NavBrand href="/">
        <img src={company.logo} class="me-3 h-6 sm:h-9" alt={company.name} />
        <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">{company.name}</span>
    </NavBrand>
    <NavHamburger />
    <NavUl {activeUrl}>
        {#each menu as {name, href, sub}, i}
            {@render navItems(name, href, sub)}
        {/each}
    </NavUl>
    <DarkMode />
</Navbar>

<style>
    
</style>