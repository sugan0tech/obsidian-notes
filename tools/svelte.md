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
   2. spread props ( just with multiple parms )
