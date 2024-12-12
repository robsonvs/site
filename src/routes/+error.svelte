<script lang="ts">
    import {page} from "$app/stores"
	import Title from "$lib/components/Title.svelte"

    import { Button } from 'flowbite-svelte'
</script>

<Title title={$page.status.toString()} header={$page.status + " - " +$page.error?.message} />

    <div class="mb-5 tracking-tight font-bold text-red-900 md:text-9xl">
        {$page.status}
    </div>
    
    <p class="mb-4 text-3xl tracking-tight font-bold text-gray-900 md:text-4xl dark:text-white">
        Something's missing.
    </p>
    <p class="mb-4 text-lg font-light text-gray-500 dark:text-gray-400">
        Sorry, we can't find that page. You'll find lots to explore on the home page.
    </p>
    
    <Button size="sm" color="red" onclick="{() => history.back()}">Back</Button>
    
    The Page requested {$page.url}, return an error "{$page.status}: {$page.error?.message}" <br/>
    <span class="font-medium">
        {$page.status + " - " +$page.error?.message}
    </span> <br/>

<style>
     
</style>