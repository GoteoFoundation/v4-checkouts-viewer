<script lang="ts">
    import UserGlimpse from "$lib/components/content/UserGlimpse.svelte";

    export let origin: string;

    let accounting: any;
    let accountingOwner = getOwner();

    async function getOwner() {
        accounting = await fetch("http://localhost:8090" + origin).then((res) =>
            res.json(),
        );

        return fetch(
            "http://localhost:8090" + accounting[accounting.ownerResource],
        ).then((res) => res.json());
    }
</script>

{#await accountingOwner then accountingOwner}
    <UserGlimpse user={accountingOwner} />
{/await}
