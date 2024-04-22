<script lang="ts">
    import MultiSelect from "$lib/components/blocks/MultiSelect.svelte";
    import Search from "$lib/components/blocks/Search.svelte";
    import ProjectGlimpse from "$lib/components/content/ProjectGlimpse.svelte";
    import * as Card from "$lib/components/ui/card";
    import * as Tabs from "$lib/components/ui/tabs";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    let searchValue: string = "";
    let searchResults: Promise<any[]> = new Promise((res) => res([]));

    export let selectedValues: any[] = [];

    function searchProjects() {
        if (searchValue === "") {
            return;
        }

        searchResults = fetch(
            "http://localhost:8090/v4/projects?" +
                new URLSearchParams([["title", searchValue]]),
        ).then((res) => res.json());
    }

    function handleChange(e: CustomEvent) {
        selectedValues = e.detail.value;

        dispatch("change", {
            value: selectedValues.map((project) => project.props),
        });
    }
</script>

<Card.Root>
    <Card.Header>
        <Card.Title>Destinos</Card.Title>
        <Card.Description>
            {selectedValues.length} destinos seleccionados
        </Card.Description>
    </Card.Header>
    <Card.Content class="pt-1 h-80 overflow-y-auto">
        <Search
            placeholder="Buscar proyectos por título"
            bind:value={searchValue}
            on:input={searchProjects}
        />
        <div class="mt-5">
            {#await searchResults then projects}
                <MultiSelect
                    bind:selected={selectedValues}
                    on:change={handleChange}
                    options={projects.map((project) => {
                        return {
                            id: project.id.toString(),
                            props: project,
                        };
                    })}
                    let:item
                >
                    <ProjectGlimpse project={item.props} />
                </MultiSelect>
            {/await}
        </div>
    </Card.Content>
</Card.Root>
