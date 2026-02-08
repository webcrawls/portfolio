<script lang="ts">
    const posts = [{
        title: 'testtesttesttest',
        when: 'today',
        by: 'kaden',
        length: '100 words'
    }]
</script>

<ul>
    {#each posts as {title, when, by, length}}
    <li>
        <p>{title}</p>
        <p>{when} by <a href="#">{by}</a> | <a href="#">link</a> | {length}</p>
    </li>
    {/each}
</ul>