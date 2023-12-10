<script>
	let promise = getUsers();
	async function getUsers() {
    // faz um request GET para endpoint /users
		const res = await fetch(`http://localhost:8000/users`);
		const text = await res.json();
		if (res.ok) { return text; } 
    else { throw new Error(text);}
	}
	function handleClick() {
		promise = getUsers();
	}
</script>

<div>

<button on:click={handleClick}> Get User </button>

{#await promise}
	<p>...loading</p>
	{:then users}
		<h1>Lista de usuários</h1>		
		{#each users as user}
    <div class="grid">
      <p>{user.id}</p>
      <p>{user.email}</p>
    </div>
		{/each}
	{:catch error}
		<p style="color:red">{error.message}</p>
{/await}

</div>

<style>
  p{ 
    font-weight: bold; 
    font-size: 2em
    
    }
  .grid{ padding: 10px; display: grid; grid-template-columns: 1fr 2fr; }
div{
  background-color: rgb(212, 253, 230);
}
</style>