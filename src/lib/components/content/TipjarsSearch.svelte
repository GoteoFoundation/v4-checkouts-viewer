<script lang="ts">
    import Search from "$lib/components/blocks/Search.svelte";

    let searchValue: string = "";
    let searchResults: Promise<any[]> = new Promise((res) => res([]));

    function searchProjects() {
        if (searchValue === "") {
            return;
        }

        searchResults = fetch(
            "http://localhost:8090/v4/tipjars?" +
                new URLSearchParams([["name", searchValue]]),
        ).then((res) => res.json());
    }
</script>

<div>
    <Search
        placeholder="Buscar botes de propina"
        bind:value={searchValue}
        on:input={searchProjects}
    />
    <div class="mt-5">
        {#await searchResults then tipjars}
            <slot {tipjars} />
        {/await}
    </div>
</div>
