<script lang="ts">
    import MultiSelect from "$lib/components/blocks/MultiSelect.svelte";
    import UserGlimpse from "$lib/components/content/UserGlimpse.svelte";
    import UsersSearch from "$lib/components/content/UsersSearch.svelte";
    import * as Card from "$lib/components/ui/card";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let selectedValues: any[] = [];

    function handleChange(e: CustomEvent) {
        selectedValues = e.detail.value;

        dispatch("change", {
            value: selectedValues.map((project) => project.props),
        });
    }
</script>

<Card.Root>
    <Card.Header>
        <Card.Title>Usuarios</Card.Title>
        <Card.Description>
            {selectedValues.length} usuarios seleccionados
        </Card.Description>
    </Card.Header>
    <Card.Content class="pt-1 h-80 overflow-y-auto">
        <UsersSearch let:users>
            <MultiSelect
                bind:selected={selectedValues}
                on:change={handleChange}
                options={users.map((user) => {
                    return {
                        id: user.username,
                        props: user,
                    };
                })}
                let:item
            >
                <UserGlimpse user={item.props} />
            </MultiSelect>
        </UsersSearch>
    </Card.Content>
</Card.Root>
