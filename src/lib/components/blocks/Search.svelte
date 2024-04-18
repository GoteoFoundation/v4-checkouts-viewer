<script lang="ts">
    import { Input } from "$lib/components/ui/input";
    import Search from "lucide-svelte/icons/search";

    export let value: string = "";
    export let id: string | undefined = undefined;
    export let placeholder: string = "Buscar...";
</script>

<div class="relative ml-auto flex-1 md:grow-0">
    <Search class="absolute left-2.5 top-2.5 h-4 w-4 text-muted-foreground" />
    <Input
        bind:value
        {id}
        {placeholder}
        type="search"
        class="w-full rounded-lg bg-background pl-8 md:w-[200px] lg:w-[336px]"
        on:input
        on:change
    />
</div>
