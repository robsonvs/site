<script>
	import Title from "$lib/components/Title.svelte";
	
	let form = {
		firstName: String(),
		lastName: String(),
		birthday: Date()
	}
</script>


<Title titleAndHeader="Home" />

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

{JSON.stringify(form)}

<style>

</style>