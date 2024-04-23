<script lang="ts">
    import MultiSelect from "$lib/components/blocks/MultiSelect.svelte";
    import ProjectGlimpse from "$lib/components/content/ProjectGlimpse.svelte";
    import ProjectsSearch from "$lib/components/content/ProjectsSearch.svelte";
    import TipjarGlimpse from "$lib/components/content/TipjarGlimpse.svelte";
    import TipjarsSearch from "$lib/components/content/TipjarsSearch.svelte";
    import * as Card from "$lib/components/ui/card";
    import * as Select from "$lib/components/ui/select";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    interface Target {
        label: string;
        value: "projects" | "tipjars";
    }

    let targets: Target[] = [
        {
            label: "Proyectos",
            value: "projects",
        },
        {
            label: "Botes de propina",
            value: "tipjars",
        },
    ];

    let selectedTarget: Target = targets[0];

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
        <Card.Title>Destinos</Card.Title>
        <Card.Description>
            {selectedValues.length} destinos seleccionados
        </Card.Description>
    </Card.Header>
    <Card.Content class="pt-1 h-80 overflow-y-auto">
        <Select.Root bind:selected={selectedTarget} onSelectedChange={() => selectedValues = []} >
            <Select.Trigger class="mb-5">
                <Select.Value placeholder="Destinos" />
            </Select.Trigger>
            <Select.Content>
                {#each targets as target}
                    <Select.Item value={target.value}>
                        {target.label}
                    </Select.Item>
                {/each}
            </Select.Content>
        </Select.Root>
        {#if selectedTarget.value === "projects"}
            <ProjectsSearch let:projects>
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
            </ProjectsSearch>
        {/if}
        {#if selectedTarget.value === "tipjars"}
            <TipjarsSearch let:tipjars>
                <MultiSelect
                    bind:selected={selectedValues}
                    on:change={handleChange}
                    options={tipjars.map((tipjar) => {
                        return { id: tipjar.id.toString(), props: tipjar };
                    })}
                    let:item
                >
                    <TipjarGlimpse tipjar={item.props} />
                </MultiSelect>
            </TipjarsSearch>
        {/if}
    </Card.Content>
</Card.Root>
