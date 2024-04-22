<script lang="ts">
    import Money from "$lib/components/content/Money.svelte";
    import ProjectGlimpse from "$lib/components/content/ProjectGlimpse.svelte";
    import TipjarGlimpse from "$lib/components/content/TipjarGlimpse.svelte";

    export let charges: string[];

    let data = Promise.all(
        charges.map(async (charge) => {
            const data = await fetch("http://localhost:8090" + charge).then(
                (res) => res.json(),
            );

            const target = await fetch(
                "http://localhost:8090" + data.target,
            ).then((res) => res.json());

            const resource = await fetch(
                "http://localhost:8090" + target[target.ownerResource],
            ).then((res) => res.json());

            return { ...data, target: { ...target, resource } };
        }),
    );
</script>

{#await data then data}
    {#each data as charge}
        <Money amount={charge.money.amount} currency={charge.money.currency} />
        &rarr;
        {#if charge.target.ownerResource === "project"}
            <ProjectGlimpse project={charge.target.resource} />
        {:else}
            <TipjarGlimpse tipjar={charge.target.resource} />
        {/if}
        <br />
        <br />
    {/each}
{/await}
