<script lang="ts">
    import BitInput from "./BitInput.svelte";

    let { base, value = [0,0,0,0,0,0,0,0], readonly=false, disabled=false } = $props();

    const handleUpdate = (el) => {
        const value = el.value
        console.log({value})
    }
</script>

<div class="binary-input" class:readonly class:disabled>
    {#each { length: base }, index}
        <BitInput {index} bind:value={value[index]} onchange={handleUpdate}/>
    {/each}
</div>

<style>
    .binary-input {
        display: flex;
        flex-direction: row;
        --gap: 2px;
        border: var(--gap) solid var(--calculator-outline);
        background-color: var(--calculator-outline);
        gap: var(--gap);
        /* gap: 1rem; */
    }

    .binary-input.disabled {
        opacity: .7;
    }
</style>