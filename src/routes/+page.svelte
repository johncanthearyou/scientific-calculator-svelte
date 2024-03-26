<script>
    const nums = [...Array(9).keys()]
    let currentNum = ""
    let previousNum = ""

    const operators = ["+", "-", "*", "/"]
    let currentOperator = ""

    function primeOperator(event) {
        previousNum = currentNum
        currentNum = ""
        currentOperator = event.target.textContent
    }

    function applyOperator() {
        switch (currentOperator) {
            case "+":
                currentNum = parseFloat(currentNum) + parseFloat(previousNum)
                break
            case "-":
                currentNum = parseFloat(currentNum) - parseFloat(previousNum)
                break
            case "*":
                currentNum = parseFloat(currentNum) * parseFloat(previousNum)
                break
            case "-":
                currentNum = parseFloat(currentNum) / parseFloat(previousNum)
                break
        }
    }
</script>

<form>
    <label>Result: <input value={currentNum}/></label>
    <br />
    <button on:click={() => currentNum = "" }>Clear</button>
    <button on:click={() => currentNum = currentNum.slice(0, -1) }>Delete</button>
    <br />
    <br />
    {#each operators as operator}
        <button on:click={primeOperator}>{operator}</button>
        <br />
    {/each}
    <br />
    {#each nums as num}
        <button on:click={() => currentNum += event.target.textContent}>{num}</button>
        <br />
    {/each}
    <br />
    <button on:click={applyOperator}>=</button>
</form>