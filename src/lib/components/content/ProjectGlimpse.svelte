<script lang="ts">
    export let project: any;

    let user = fetch('http://localhost:8090' + project.owner).then(res => res.json());
</script>

<p>{project.title}</p>
<p class="text-sm text-muted-foreground">
    <code>#</code>{project.id}
</p>
<p class="text-sm text-muted-foreground">
    <code>por</code>
    {#await user then user}
        @{user.username}
    {/await}
</p>

