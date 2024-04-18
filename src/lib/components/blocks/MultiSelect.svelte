<script lang="ts">
    import { createEventDispatcher } from "svelte";
    import MultiSelectOption from "$lib/components/blocks/MultiSelectOption.svelte";

    interface Item {
        id: string;
        props: any
    }

    export let items: Item[];

    export let selected: Item[] = [];
    let selectableItems: Item[] = items.filter(
        (p: Item) => !selected.map((s: Item) => s.id).includes(p.id),
    );

    const dispatch = createEventDispatcher();

    function addSelection(e: CustomEvent) {
        const id = e.detail.id;

        selected = [...selected, items.filter((p: Item) => p.id === id)[0]];
        selectableItems = selectableItems.filter((p: Item) => p.id !== id);

        dispatch("change", { value: selected });
    }

    function removeSelection(e: CustomEvent) {
        const id = e.detail.id;
        const item = items.filter((p: Item) => p.id === id)?.[0];

        selected = selected.filter((p: Item) => p.id !== id);

        if (typeof item !== "undefined") {
            selectableItems = [...selectableItems, item].sort(
                (a: Item, b: Item) =>
                    new Number(b.id).valueOf() - new Number(a.id).valueOf(),
            );
        }

        dispatch("change", { value: selected });
    }
</script>

{#each selected as item, i (item.id)}
    <MultiSelectOption
        id={item.id}
        checked={true}
        on:checked={addSelection}
        on:unchecked={removeSelection}
    >
        <slot {item} index={i} />
    </MultiSelectOption>
{/each}
{#each selectableItems as item, i (item.id)}
    <MultiSelectOption
        id={item.id}
        checked={false}
        on:checked={addSelection}
        on:unchecked={removeSelection}
    >
        <slot {item} index={i} />
    </MultiSelectOption>
{/each}
