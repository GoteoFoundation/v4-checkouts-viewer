<script lang="ts">
    import { createEventDispatcher } from "svelte";
    import Checkbox from "../ui/checkbox/checkbox.svelte";
    import Label from "../ui/label/label.svelte";

    export let id: string;
    export let title: string;
    export let subtitle: string;
    export let checked: boolean = false;

    const dispatch = createEventDispatcher();

    $: if (checked) {
        dispatch("checked", { id });
    } else {
        dispatch("unchecked", { id });
    }
</script>

<Label for={id}>
    <div class="items-top flex space-x-2 mt-5 hover:cursor-pointer">
        <Checkbox {id} bind:checked />
        <div class="grid gap-1.5 leading-none">
            {title}
            <p class="text-sm text-muted-foreground">
                {subtitle}
            </p>
        </div>
    </div>
</Label>
