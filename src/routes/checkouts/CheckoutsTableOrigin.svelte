<script lang="ts">
    import UserGlimpse from "$lib/components/content/UserGlimpse.svelte";

    export let origin: any;

    let owner = fetch(
        "http://localhost:8090" + origin[origin.ownerResource],
    ).then((res) => res.json());
</script>

{#await owner}
    <p>Cargando datos...</p>
{:then owner}
    <UserGlimpse user={owner} />
{/await}
