<script>
	let name = 'world';
	import Counter from './Counter.svelte';
	import Nested from './Nested.svelte';
	import Info from './Info.svelte';


	const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};

	let user = { loggedIn: false };

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}

	let x = 7;

	let cats = [
		{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
		{ id: 'z_AbfPXTKms', name: 'Maru' },
		{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
	];


	import Thing from './Thing.svelte';
	let things = [
		{ id: 1, color: 'darkblue' },
		{ id: 2, color: 'indigo' },
		{ id: 3, color: 'deeppink' },
		{ id: 4, color: 'salmon' },
		{ id: 5, color: 'gold' }
	];

	function thingsHandleClick() {
		things = things.slice(1);
	}
	
	function randomNumber(){
	return new Promise((resolve, reject) => {
		setTimeout(()=>{
			resolve({
				ok : true,
				text : Math.random()*100
			});

		},3000)
	})
	}
	async function getRandomNumber() {
		const res = await randomNumber();
		const text = res.text;

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}

	let promise = getRandomNumber();

	function randomHandleClick() {
		promise = getRandomNumber();
	}

</script>

<h1>Hello {name}!</h1>
<Counter />
<Nested answer={42}/>
<Info {...pkg}/>

{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{:else}
	<button on:click={toggle}>
		Log in
	</button>
{/if}


{#if x > 10}
	<p>{x} is greater than 10</p>
{:else if 5 > x}
	<p>{x} is less than 5</p>
{:else}
	<p>{x} is between 5 and 10</p>
{/if}


{#each cats as {id, name}, i}
	<li><a target="_blank" href="https://www.youtube.com/watch?v={id}">
		{i + 1}: {name}
	</a></li>
{/each}


<button on:click={thingsHandleClick}>
	Remove first thing
</button>

{#each things as thing (thing.id)}
	<Thing current={thing.color}/>
{/each}

<button on:click={randomHandleClick}>
	generate random number
</button>

{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}