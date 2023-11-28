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
```jsx
<button on:click|once = { () => alert('clicked')} ( also be passed as array func (inline))
	click me
</button> 
// some interesting events
self // ( only if event.target is the element itself)
trusted // only triggers if the person interacted not a js code

```
8. binding input
	1. some refs
		1. https://learn.svelte.dev/tutorial/numeric-inputs
		2. https://learn.svelte.dev/tutorial/checkbox-inputs
		3. https://learn.svelte.dev/tutorial/select-bindings
```html
<input bind:value={name}> assigns the value to name also svelte automatically handles the type.
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
4. Aparently event's has to be forwarded 
	1. let's say we have a button, upon clicking it has to be refelected to the parent .
	2. in this scenario we have to denote the passage in inbetween components as well\
```jsx ( leaf component ) Inner.svelte
<script> 
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('mesage', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>

```

```jsx // inbetween component Outer.svelte
<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:mesage/>
```

```jsx // parent component which handles the event ( app.svelte )
<script>
	import Outer from './Outer.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Outer on:mesage={handleMessage} />
```




## Store
- the web db ( globally accessible variable across the frontend )
- create a store.js  and populate the necessary objects
```js
import { writable } from 'svelte/store'
let count = writable(0); // comes with .set and .update methods
```

```jsx 
//on other component
<script>
	import { count } from 'store.js'
	function some() {
		count.update((n) -> n + 1);
	}
	function clear(){
		count.set(0)
	}
</script>
```
- if we want to use the count ( need to be a subscriber ), like associate with the local state.
```html
<script>
	import { count } from 'store.js'
	let local_store_count;
	count.subscribe((value) -> {
		local_store_count = value  // when ever the count is update the loca_store_count updates and reflected in current component
	})
</script>
```
- ==Not it's to know without ending subscription will ends up in memory leak== need to call the unsubscribe ( which will be returned from the subscribe call )
```html
<script>
	import { count } from 'store.js'
	let local_store_count;
	let unsubscribe = count.subscribe((value) -> {
		local_store_count = value  
	})
	onDestroy(unsubscribe) // called after the components ends it's life
</script>
```
- these above system is kinda boiler plate so with svelte we can use $count directly and it will do the post processing ( including subscribe and unsubscribe) ==AutoSubscription==
```html
<script>
	import { count } from 'store.js'
</script>
<p> {$count} </p> // directly being used
```
- We cal also make readonly stores 
  Not all stores should be writable by whoever has a reference to them. For example, you might have a store representing the mouse position or the user's geolocation, and it doesn't make sense to be able to set those values from 'outside'. For those cases, we have _readable_ stores.

Open `stores.js`. The first argument to `readable` is an initial value, which can be `null` or `undefined` if you don't have one yet. The second argument is a `start` function that takes a `set` callback and returns a `stop` function. The `start` function is called when the store gets its first subscriber; `stop` is called when the last subscriber unsubscribes

```js
import { readable } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
	const interval = setInterval(() => {
		set(new Date());
	}, 1000)

	return function stop() {
		clearInterval(interal);
	};
});

```

```html
<script>
	import { time } from './stores.js';

	const formatter = new Intl.DateTimeFormat(
		'en',
		{
			hour12: true,
			hour: 'numeric',
			minute: '2-digit',
			second: '2-digit'
		}
	);
</script>

<h1>The time is {formatter.format($time)}</h1>
```
- Derived store
```js
import { readable, derived } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return function stop() {
		clearInterval(interval);
	};
});

const start = new Date();

export const elapsed = derived(
	time,
	($time) => Math.round(($time - start)/1000)
);
```
- custom stores ( has built in methods instead of exposing set and update )
```js
import { writable } from 'svelte/store';

function createCount() {
	const { subscribe, set, update } = writable(0);

	return {
		subscribe,
		increment: () => update((n) => n + 1),
		decrement: () => update((n) => n - 1),
		reset: () => set(0)
	};
}

export const count = createCount();

```