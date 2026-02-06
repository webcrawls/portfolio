<script lang="ts">
    import BinaryInput from "$lib/binary-visualizer/BinaryInput.svelte";

    // Basic operations
    const addition = (primary, secondary) => {
        /* How the fuck would I do carryovers in this, i'd need state?
        well, we can determine the base from the length of primary and secondary, make em bit arrays of 8 ig?
        */

        if (primary.length != secondary.length)
            throw new Error(
                "Bits are not of same length! ('bit field length'?)",
            );
        const base = primary.length;
    };

    const not = (primary) => {
        const base = primary.length;
        const result = new Array(base);
        for (let index = 0; index < base; index++) {
            if (primary[index] === 0) {
                result[index] = 1;
            } else {
                result[index] = 0;
            }
        }
        return result;
    };

    const and = (primary, secondary) => {
        const primaryBase = primary.length;
        const secondaryBase = secondary.length;

        const base = Math.max(primaryBase, secondaryBase);

        const result = new Array(base);
        for (let index = 0; index < base; index++) {
            const first = index < primary.length ? primary[index] : 0;
            const second = index < secondary.length ? secondary[index] : 0;

            if (first === 1 && second === 1) {
                result[index] = 1;
            } else {
                result[index] = 0;
            }
        }
        return result;
    };

    const or = (primary, secondary) => {
        const primaryBase = primary.length;
        const secondaryBase = secondary.length;

        const base = Math.max(primaryBase, secondaryBase);

        const result = new Array(base);
        for (let index = 0; index < base; index++) {
            const first = index < primary.length ? primary[index] : 0;
            const second = index < secondary.length ? secondary[index] : 0;

            if (first === 1 || second === 1) {
                result[index] = 1;
            } else {
                result[index] = 0;
            }
        }
        return result;
    };

    const xor = (primary, secondary) => {
        const primaryBase = primary.length;
        const secondaryBase = secondary.length;

        const base = Math.max(primaryBase, secondaryBase);

        const result = new Array(base);
        for (let index = 0; index < base; index++) {
            const first = index < primary.length ? primary[index] : 0;
            const second = index < secondary.length ? secondary[index] : 0;

            if (
                (first === 1 && second === 1) ||
                (first === 0 && second === 0)
            ) {
                result[index] = 0;
            } else {
                result[index] = 1;
            }
        }
        return result;
    };

    const compute = (primary, secondary, operation) => {
        if (primary === null) return;

        const operations = {
            not, and, or, xor,
        };

        console.log(operation);

        return operations[operation](primary, secondary) || null;
    };

    // State & input/front-end logic
    const parseInput = (input) => {
        const isNumeric = (input) => {
            // Regards to guy on StackOverflow: https://stackoverflow.com/a/175787
            if (typeof str != "string") return false; // we only process strings!
            return (
                !isNaN(str) && // use type coercion to parse the _entirety_ of the string (`parseFloat` alone does not do this)...
                !isNaN(parseFloat(str))
            ); // ...and ensure strings of whitespace fail
        };
        if (!isNumeric) {
            throw new Error("eee errr");
        }

        const length = input.length;
        const result = new Array(length);
        for (let index = 0; index < length; index++) {
            const value = input[index];
            // console.log({value, index, input})
            if (value == "1") {
                result[index] = 1;
            } else if (value == "0") {
                result[index] = 0;
            }
        }
        return result;
    };

    let primaryInput: string = $state("1111");
    let secondaryInput: string = $state("1111");
    let baseInput: string = $state("8");

    let base = $derived.by(() => {
        return Math.max(
            parseInt(baseInput),
            primaryInput.length,
            secondaryInput.length,
        );
    });

    let primary = $derived.by(() => parseInput(primaryInput));
    let secondary = $derived.by(() => parseInput(secondaryInput));

    let operation: string = $state("not");

    let result = $derived.by(() => {
        return compute(primary, secondary, operation);
    });
</script>

<header>
    <h1>Logical Operations Visualizer</h1>
</header>

<article>
    <section id="binary-input">
        <div class="binary">
            <input
                type="text"
                bind:value={primaryInput}
                placeholder="binary number 1"
            />
            <p>Base: {primary.length}</p>
        </div>
        <div class="binary">
            <input
                type="text"
                bind:value={secondaryInput}
                placeholder="binary number 2"
                disabled={operation === "not"}
            />
            <p>Base: {secondary.length}</p>
        </div>
    </section>

    <select bind:value={operation}>
        <option value="not">NOT</option>
        <option value="and">AND</option>
        <option value="or">OR</option>
        <option value="xor">XOR</option>
        <option disabled="true">Bi-direction</option>
        <option disabled="true">Implication</option>
        <option disabled="true">Addition</option>
        <option disabled="true">Subtraction</option>
    </select>

    <input type="text" readonly="true" value={result} />
</article>
<hr />

<article class="calculator">
    <!-- <input type="range" max="16" min="1" bind:value={baseInput} /> -->
    <div class="input-group">
        <BinaryInput {base} bind:value={primary} />
        <div class="input-text">{operation}</div>
        {#if operation !== "not"} 
            <BinaryInput {base} disabled={operation === "not"} bind:value={secondary} />
        {/if}
        <div class="input-text">equals</div>
        <BinaryInput {base} bind:value={result} readonly={true} />
    </div>
</article>

<blockquote>
    This was an exercise in implementing the logical operators in a programming
    language (JavaScript).
    <br />And then making it look cool.
    <br />From Module 1 of my Computer Math Fundementals.
    <br />Hopefully will be able to implement Addition & Subtraction soon. I guess, how hard is it to implement the by-hand method(s) we learned?
    <br />And then some way to encode a boolean expression and run the k-map
    stuff would be cool. But i don't know how difficult that is, i guess you'd have to make a parser?
</blockquote>

<style>
    :global(*) {
        box-sizing: border-box;
    }

    :global(:root) {
        --calculator-bg: rgb(70, 70, 70);
        --calculator-outline: #999999;
        --calculator-input-bg: #bcbcbc;
        --calculator-input-off-bg: #bcbcbc;

        --calculator-text-outer: rgb(218, 218, 218);
        --calculator-text-input: black;
        --calculator-text-input-off: black;
        --calculator-text-input-alt: gray;
    }

    .calculator {
        background-color: var(--calculator-bg);
        width: min-content;
        padding: 4rem;
    }

    .input-text {
        color: var(--calculator-text-outer)
    }

    #binary-input {
        display: flex;
        flex-direction: row;
        gap: 2rem;

        p {
            margin-top: 0.5rem;
            font-size: 12px;
            color: rgb(48, 48, 48);
        }
    }

    .input-text {
        width: 100%;
        display: flex;
        justify-content: center;
    }

    div.input-group {
        width: min-content;
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }
</style>
