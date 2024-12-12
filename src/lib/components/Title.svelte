<script lang="ts">
    interface Props { 
        title?: string, 
        header?: string,
        titleAndHeader?: string    
    }
            
    let { title, header, titleAndHeader }: Props = $props()
</script>

<svelte:head>
    <title>{titleAndHeader !== undefined ? titleAndHeader : title}</title>
</svelte:head>

<h1>{titleAndHeader !== undefined ? titleAndHeader : header}</h1> <hr/>

<style>
    
</style>