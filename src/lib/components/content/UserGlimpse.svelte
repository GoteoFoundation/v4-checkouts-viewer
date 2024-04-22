<script lang="ts">
    export let user: any;
</script>

<p>
    {user.name}
    <span class="text-muted-foreground">
        ({user.email})
    </span>
</p>
<p class="text-sm text-muted-foreground">
    <code>#</code>{user.id}
</p>
<p class="text-sm text-muted-foreground">
    <code>@</code>{user.username}
</p>
