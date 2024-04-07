<script>
	// Variables
	const calculatorButtons = [
		'7', '8', '9', ' / ',
		'4', '5', '6', ' * ',
		'1', '2', '3', ' - ',
		'0', '.', '=', ' + '
	];
	let expression = '';
	let seenDecimal = false;

	// Functions
	function backspace() {
		const endIndex = (expression[expression.length - 1] == ' ' ? -3 : -1);
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
			alert('ERROR!\nTo avoid ambiguity, simple calculator can only handle two argument expressions using of of the 4 arithmetic operators:\nargA + argB,\nargA / argB,\nargA * argB,\nargA / argB');
			return expression;
		}

		// Extract tokens and evaluate
		const [operator] = expression.match(/ [+\-*/] /);
		const [numA, numB] = expression.match(/-?([0-9]*[.])?[0-9]+/g);
		return applyOperator(operator, numA, numB);
	}

	function updateExpression(event) {
		if ([" + ", " - ", " * ", " / ", " = "].includes(event.target.textContent)) {
			seenDecimal = false;
		}

		// Update expression
		if (seenDecimal && event.target.textContent==".") {
			alert("ERROR!\nYou cannot use a decimal point (.) twice in a single argument.\nPlease try again")
		} else if (event.target.textContent == '=') {
			expression = evaluateExpression();
		} else {
			expression += event.target.textContent;
		}

		// Update seenDecimal
		if (event.target.textContent == ".") {
			seenDecimal = true;
		}
	}
</script>

<div class="p-1 grid grid-cols-4 gap-1 w-64 h-64">
	<strong class="col-span-3">Scientific Calculator</strong>
	<input class="bg-slate-600 rounded outline outline-gray-500 text-4xl col-span-3 row-span-2" value={expression} />
	<button class="col-start-4 bg-blue-500 text-base rounded" on:click={backspace}>Back</button>
	<button class="col-start-4 bg-red-500 text-base rounsded" on:click={() => (expression = '')}>Clear</button>
	{#each calculatorButtons as calculatorButton}
		<button class="bg-slate-700 rounded" on:click={updateExpression}
			>{calculatorButton}</button
		>
	{/each}
</div>
