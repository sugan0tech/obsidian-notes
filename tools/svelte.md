A well designed ui framework :)

## History.
- My past ui tool experience. Most of it were in react in context, props and redux things. Kinda got overwhelmed with the flow and design of react.
- On the quest of finding a well suited frontend ui ( for the backend guy ), found svelte as well suited


## Things that i like about svelte
1. easy peasy state
   ```html
   <script>
	   let count = 3; ( behaves as react state )
   </script>
   ```
2. Reactive Declarations
   ```html
	<script>
		let count = 1;
		$: double = count * 2; // updated when ever the count changes
		// also supports block 
		$: {
			console.log(`the count is ${count}`);
		}
		// condition blocks
		$: if( count > 10){
			console.log(`well now it's bigger than 10`)
		}
		function addCount(){
			count += 1;
		}
	</script>
	<button on:click={addCount}>
		Clicked {count}
		The reactive double {double}
	</button>
   ```
3. Props #props
      ```html 
	     // Nexted svelte
	    <script>
			export let prop = 23 // default val; // just make a exported variable
	    </script>
		<p> using that {prop} </p>
		// App.svelete
	    <script>
			import Nexted from './Nexted';
	    </script>
		<Nexted prop={42} /> // just specify the name and pass it
		<Nexted> // default val will popup
      ```
4. Conditional logics
```jsx
{#if cond}
	<p> to be displayed upon condition </p>
{:else if cond}
{:else }
{/if} 
```
5. For each
```html
<script>
	let colors = ["red", "blue", "orange"];
</script>
<div>
	{#each colors as color}
		<p>color {color}</p>
	{/each}
</div>
```
6. updation in each loop
	1. let's say some deletion occurs in colors then it removes the last <p\> color </p\> and updates the value.
	2. For reactive behaviour this shouldn't be for all ( might be re render the exact component where it's been modified ). So in this scenario we have to use the id specifier ( it tells svelte to point on which dom element  to modify ).
```html
<script>
	import Thing from './Thing.svelte';

	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	];

	function handleClick() {
		things = things.slice(1);
	}
</script>

<button on:click={handleClick}>
	Remove first thing
</button>

{#each things as thing (thing.id)}
	<Thing name={thing.name} />
{/each}
```
```html
<script>
	const emojis = {
		apple: '🍎',
		banana: '🍌',
		carrot: '🥕',
		doughnut: '🍩',
		egg: '🥚'
	};

	// the name is updated whenever the prop value changes...
	export let name;

	// ...but the "emoji" variable is fixed upon initialisation
	// of the component because it uses `const` instead of `$:`
	const emoji = emojis[name];
</script>

<p>{emoji} = {name}</p>
```
 7. events
```html
<button on:click|once = { () => alert('clicked')} ( also be passed as array func (inline))
	click me
</button> 
```



## General concepts
1. Updating ==arrays and objects ==
   let's say we are using a array inside a html and updating it, this wont trigger svelte's reactivity for the methods like slice, split, pop, push and shift.
   ```html
	<script>
		let numbers = [1, 2, 3];
		function addNumbers(){
			numbers.push(numbers.length + 1);
			numbers = numbers; // only be reactive via assignment
		}
		// or in better way 
		function addNumbersAnother(){
			numbers = [...numbers, numbers.length + 1];
		}
		$: sum = numbers.reduce((total, currentNumber) => total + currentNumber, 0);
	</script>
	<p>{numbers.join('+')} = {sum}}</p>

	<button on:click={addNumbers}>
		add a number
	</button>
   ```
   2. spread props ( just with multiple parms ) #props
```jsx
<script>
	const pkg = {
	name: "name",
	speed: 3
	};
</script>
<PackageInfo 
	name={pkg.info}
	speed={pkg.speed}
/>
// or 
<PackageInfo {...pkg} />
```
3. await block
```html
<script>
	import { getRandomNumber } from './utils.js';

	let promise = getRandomNumber();
	function handleClick(){
	promise = getRandomNumber()
	};
</script>
{#await promise}
	<p> ...waiting </p>
{:then number}
	<p>The number is </p>
{:catch error}
	<p style="color:red"> {error.message} </p>
{/await}
```
```js // util.js
export async function getRandomNumber(){
	// some api call may be
	await sleep(1000);
	return 5
}
```