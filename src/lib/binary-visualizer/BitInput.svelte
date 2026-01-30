<script lang="ts">
    let { index, value = $bindable(), onchange } = $props();
</script>

<div class="bit-input">
    <input type="text" bind:value maxlength="1" {onchange} />
    <div class="bit-cover">
        <p class='bit-index'>{index}</p>
        <p class='bit-value'>{value}</p>
    </div>
</div>

<style>
    .bit-input {
        --scale: 64px;
        width: var(--scale);
        height: var(--scale);

        position: relative;

        background-color: var(--calculator-input-bg);
    }

    input {
        width: var(--scale);
        height: var(--scale);
        background-color: transparent;
        padding: 0;
        border: none;
        outline: none;
        color: transparent;
        position: absolute;
    }

    .bit-cover {
                z-index: 1;
        pointer-events: none;
        position: relative;
        width: 100%;
        height: 100%;

        & .bit-value {
            position: absolute;
            padding: 0;
            width: 16px;
            height: 16px;
top: 0;
left: 0;
right: 0;
bottom: 0;
margin: auto;

width: min-content;
        }

        & .bit-index {
            
            width: 16px;
            height: 16px;
            font-size: 12px;
            opacity: 0.5;
            position: absolute;
            padding: 0;
            margin: 0;

top: 4px;
left: 4px;


            width: min-content;
        }
    }
</style>
