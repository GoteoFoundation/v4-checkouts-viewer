<script lang="ts">
    import Search from "$lib/components/blocks/Search.svelte";

    let searchValue: string = "";
    let searchResults: Promise<any[]> = new Promise((res) => res([]));

    function searchProjects() {
        if (searchValue === "") {
            return;
        }

        searchResults = fetch(
            "http://localhost:8090/v4/projects?" +
                new URLSearchParams([["title", searchValue]]),
        ).then((res) => res.json());
    }
</script>

<div>
    <Search
        placeholder="Buscar proyectos"
        bind:value={searchValue}
        on:input={searchProjects}
    />
    <div class="mt-5">
        {#await searchResults then projects}
            <slot {projects} />
        {/await}
    </div>
</div>
