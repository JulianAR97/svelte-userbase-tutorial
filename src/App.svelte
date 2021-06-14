<script>
	let username, password
	// will be set by userbase
	let userObj = null
	const userbase = window.userbase

	// Userbase initializer
	let authProm = userbase.init({appId: 'a4be3741-e947-4d50-a6fc-f64551c5ef2e'})
		.then(({user}) => userObj = user)
	
	// Very simple authentication functions
	const signIn = () => authProm = userbase.signIn({username, password}).then(user => userObj = user)
	const signOut = () => authProm = userbase.signOut().then(() => userObj = null)
	const signUp = () => authProm = userbase.signUp({username, password}).then(user => userObj = user )
</script>


<!---Control flow in Svelte is very oranized and easy to read-->
{#await authProm}
	Waiting...
{:then}
	<!--Code blocks in Svelte are easier to read than functions conditionally returning JSX in my opinion-->
	{#if !userObj}
		<!--bind value will automatically set value of variables username and password to the input values -- beautiful-->
		<input placeholder="username" type="text" bind:value={username}><br>
		<input placeholder="password" type="password" bind:value={password}><br>
		<button on:click={signIn}>Sign In</button>
		<button on:click={signUp}>Sign Up</button>	
	{:else}
		<h3>Hello, {userObj.username}!</h3>
		<button on:click={signOut}>Sign Out</button>
	{/if}

{:catch error}
	<h3 class="error">{error.message}</h3>
{/await}

<!--Style tags in each 'component' significantly reduce css headaches-->
<style>
	.error {
		color: red;
	}

	h3 {
		color: blue;
	}
</style>