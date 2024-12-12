<script lang="ts">
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger, Dropdown, DropdownItem, DropdownDivider, DarkMode } from 'flowbite-svelte';
    import { ChevronDownOutline } from 'flowbite-svelte-icons';
    import { page } from '$app/stores';
    
    let activeUrl = $derived($page.url.pathname)

    let menu = [
        { name: 'Home', href: '/' },
        { name: 'Portfolio', href: '/portfolio' },
        { name: 'About', href: '/about' },
        { name: 'Admin', sub: [
            {name: 'Proprieties', href: '/propeties'},
            {name: 'Tenents', href: '/tenents'}
        ]}
    ]

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

{#snippet navItems(name: string, href: any, sub: any)}
    {#if href}
        <NavLi {href}>{name}</NavLi>
    {:else} 
        <NavLi class="cursor-pointer">
            {name}<ChevronDownOutline class="w-6 h-6 ms-2 text-primary-800 dark:text-white inline" />
        </NavLi>
        <Dropdown {activeUrl} class="w-44 z-20">
            {#each sub as {name, href}, i}
                <DropdownItem {href}>{name}</DropdownItem>
            {/each}
        </Dropdown>
    {/if}
{/snippet}

<Navbar>
    <NavBrand href="/">
        <img src="" class="me-3 h-6 sm:h-9" alt="Company Logo" />
        <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">Company Name</span>
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