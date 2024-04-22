<script lang="ts">
    import Search from "$lib/components/blocks/Search.svelte";
    import { Skeleton } from "$lib/components/ui/skeleton";

    let searchValue: string = "";
    let searchResults: Promise<any[]> = new Promise((res) => res([]));

    async function searchUsers() {
        if (searchValue === "") {
            return;
        }

        if (searchValue.startsWith("#")) {
            const users = await searchUsersById(searchValue.slice(1));

            if (users.length === 1) {
                searchResults = new Promise((res) => res(users));
                return;
            }
        }

        if (searchValue.startsWith("@")) {
            const users = await searchUsersByUsername(searchValue.slice(1));

            if (users.length > 0) {
                searchResults = new Promise((res) => res(users));
                return;
            }
        }

        searchResults = fetch(
            "http://localhost:8090/v4/users?" +
                new URLSearchParams([["query", `%${searchValue}%`]]),
        ).then((res) => res.json());
    }

    async function searchUsersById(id: string) {
        if (id === "") {
            return [];
        }

        const res = await fetch("http://localhost:8090/v4/users/" + id);

        if (res.status === 404) {
            return [];
        }

        const user = await res.json();

        return [user];
    }

    async function searchUsersByUsername(username: string) {
        if (username === "") {
            return [];
        }

        const res = await fetch(
            "http://localhost:8090/v4/users?" +
                new URLSearchParams([["username", username]]),
        );

        return await res.json();
    }
</script>

<div>
    <Search
        placeholder="Buscar usuarios"
        bind:value={searchValue}
        on:input={searchUsers}
    />
    <div class="mt-5">
        {#await searchResults}
            <div class="flex items-center space-x-4">
                <Skeleton class="h-12 w-12 rounded-full" />
                <div class="space-y-2">
                    <Skeleton class="h-4 w-[250px]" />
                    <Skeleton class="h-4 w-[200px]" />
                </div>
            </div>
        {:then users}
            <slot {users} />
        {/await}
    </div>
</div>
