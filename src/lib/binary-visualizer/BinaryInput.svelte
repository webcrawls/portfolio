<script lang="ts">
    import BitInput from "./BitInput.svelte";

    let { base, value = [0,0,0,0,0,0,0,0] } = $props();

</script>

<div class="binary-input">
    {#each { length: base }, index}
        <BitInput {index} bind:value={value[index]} onchange={() => {}}/>
    {/each}
</div>

<style>
    .binary-input {
        display: flex;
        flex-direction: row;
        gap: 1rem;
    }
</style>