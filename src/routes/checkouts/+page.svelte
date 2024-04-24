<script lang="ts">
    import CheckoutsTable from "./CheckoutsTable.svelte";
    import UsersSelect from "./UsersSelect.svelte";
    import TargetSelect from "./TargetSelect.svelte";
    import Filters from "./Filters.svelte";
    import Downloadable from "$lib/components/blocks/Downloadable.svelte";

    let checkouts: Promise<any[]>;

    let usersParam: any[][] = [];
    let projectsParam: any[][] = [];
    let filtersParams: any[][] = [];

    $: url =
        "http://localhost:8090/v4/gateway_checkouts?" +
        new URLSearchParams([
            ...projectsParam,
            ...usersParam,
            ...filtersParams,
        ]);

    $: checkouts = fetch(url).then((res) => res.json());

    function searchCheckoutsByProject(projects: any[]) {
        projectsParam = projects.map((project) => {
            return ["charges.target[]", project.accounting];
        });
    }

    function searchCheckoutsByUsers(users: any[]) {
        usersParam = users.map((user) => {
            return ["origin[]", user.accounting];
        });
    }

    function addFiltersParams(filters: any[][]) {
        filtersParams = filters;
    }
</script>

<div class="space-y-10">
    <div class="grid grid-cols-12 gap-10">
        <div class="col-span-3">
            <UsersSelect
                on:change={(e) => searchCheckoutsByUsers(e.detail.value)}
            />
        </div>
        <div class="col-span-3">
            <TargetSelect
                on:change={(e) => searchCheckoutsByProject(e.detail.value)}
            />
        </div>
        <div class="col-span-6">
            <Filters on:change={(e) => addFiltersParams(e.detail.value)} />
        </div>
    </div>
    {#await checkouts}
        <h1>Cargando resultados...</h1>
    {:then checkouts}
        <CheckoutsTable {checkouts} />
        <Downloadable {url} options={{ headers: { Accept: "text/csv" } }}>
            Exportar a CSV
        </Downloadable>
    {/await}
</div>
