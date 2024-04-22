<script lang="ts">
    import CheckoutsTable from "./CheckoutsTable.svelte";
    import UsersSelect from "./UsersSelect.svelte";
    import TargetSelect from "./TargetSelect.svelte";

    let checkouts: Promise<any[]>;

    let usersParam: any[][] = [];
    let projectsParam: any[][] = [];

    $: checkouts = fetch(
        "http://localhost:8090/v4/gateway_checkouts?" +
            new URLSearchParams([...projectsParam, ...usersParam]),
    ).then((res) => res.json());

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
    </div>
    {#await checkouts}
        <h1>Cargando resultados...</h1>
    {:then checkouts}
        <CheckoutsTable {checkouts} />
    {/await}
</div>
