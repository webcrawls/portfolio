<script lang="ts">
    const posts = [
        {
            title: "testtesttesttest",
            when: "today",
            by: "kaden",
            length: "100 words",
        },
    ];
</script>

<ul>
    {#each posts as { title, when, by, length }}
        <li>
            <p><a href="#">{title}</a></p>
            <p>
                {when} by <a href="#">{by}</a> | <a href="#">link</a> | {length}
            </p>
        </li>
    {/each}
</ul>

<style>
    ul {
        display: flex;
        flex-direction: column;
        padding-left: 1.5rem;
        list-style-type:decimal;
    }

    li a {
        color: inherit;
    }
</style>