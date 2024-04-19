<script lang="ts">
    import { createEventDispatcher } from "svelte";
    import MultiSelectOption from "$lib/components/blocks/MultiSelectOption.svelte";

    interface Item {
        id: string;
        props: any;
    }

    export let options: Item[] = [];

    let optionsIds: string[];
    $: optionsIds = options.map((option: Item) => option.id);

    export let selected: Item[] = [];

    let selectedIds: string[];
    $: selectedIds = selected.map((selection: Item) => selection.id);

    let mixedOptions: Item[] = [...selected, ...options];
    let allOptions: Item[] = [
        ...new Set(mixedOptions.map((o: Item) => o.id)),
    ].map((id: string) => mixedOptions.filter((o: Item) => o.id === id)[0]);

    function isSelected(id: string) {
        return selectedIds.includes(id);
    }

    const dispatch = createEventDispatcher();

    function addSelection(e: CustomEvent) {
        const id = e.detail.id;

        selected = [...selected, options.filter((o: Item) => o.id === id)[0]];

        dispatch("change", { value: selected });
    }

    function removeSelection(e: CustomEvent) {
        const id = e.detail.id;

        selected = selected.filter((o: Item) => o.id !== id);

        dispatch("change", { value: selected });
    }
</script>

{#each allOptions as item, i (item.id)}
    <MultiSelectOption
        id={item.id}
        checked={isSelected(item.id)}
        on:checked={addSelection}
        on:unchecked={removeSelection}
    >
        <slot {item} index={i} />
    </MultiSelectOption>
{/each}
