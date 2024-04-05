<script>
	// Variables
	const calculatorButtons = [
		'7', '8', '9', ' / ',
		'4', '5', '6', ' * ',
		'1', '2', '3', ' - ',
		'0', '.', '=', ' + '
	];
	let expression = '';

	// Functions
	function backspace() {
		const endIndex = expression[expression.length - 1] == ' ' ? -3 : -1;
		expression = expression.slice(0, endIndex);
	}

	function applyOperator(operator, numA, numB) {
		switch (operator) {
			case ' + ':
				return parseFloat(numA) + parseFloat(numB);
			case ' - ':
				return parseFloat(numA) - parseFloat(numB);
			case ' * ':
				return parseFloat(numA) * parseFloat(numB);
			case ' / ':
				return parseFloat(numA) / parseFloat(numB);
		}
	}

	function evaluateExpression() {
		// Validate expression as valid using regex (no parentheses)
		const validatedExpression = expression.match(
			/^(-?([0-9]*[.])?[0-9]+) [+\-*/] (-?([0-9]*[.])?[0-9]+)$/g
		);
		if (validatedExpression == null) {
			alert(`${expression} is an invalid arithmetic expression.`);
			return;
		}

		// Extract tokens and evaluate
		const [numA, numB] = expression.match(/-?([0-9]*[.])?[0-9]+/g);
		const [operator] = expression.match(/ [+\-*/] /);
		expression = applyOperator(operator, numA, numB);
	}

	function updateExpression(event) {
		if (event.target.textContent != '=') {
			expression += event.target.textContent;
		} else {
			evaluateExpression();
		}
	}
</script>

<div class="w-1/2 max-w-80 mx-auto py-2 grid grid-cols-4 gap-1">
	<input class="rounded outline outline-gray-500 text-4xl col-span-3 row-span-2" value={expression} />
	<button class="col-start-4 bg-blue-500 text-white text-base rounded" on:click={backspace}>Back</button>
	<button class="col-start-4 bg-red-500 text-white text-base rounded" on:click={() => (expression = '')}>Clear</button>
	{#each calculatorButtons as calculatorButton}
		<button class="bg-gray-500 text-white rounded" on:click={updateExpression}
			>{calculatorButton}</button
		>
	{/each}
</div>
