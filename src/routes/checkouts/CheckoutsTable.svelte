<script lang="ts">
    import {
        createRender,
        createTable,
        Render,
        Subscribe,
    } from "svelte-headless-table";
    import { readable } from "svelte/store";
    import * as Table from "$lib/components/ui/table";
    import CheckoutsTableOrigin from "./CheckoutsTableOrigin.svelte";
    import CheckoutsTableStatus from "./CheckoutsTableStatus.svelte";

    type Checkout = {
        id: string;
        origin: string;
        status: "pending" | "charged";
        gateway: string;
        charges: any[];
    };

    export let checkouts: Checkout[];

    const table = createTable(readable(checkouts));

    const columns = table.createColumns([
        table.column({
            accessor: "id",
            header: "ID",
        }),
        table.column({
            header: "Origin",
            accessor: "origin",
            cell: ({ value }) =>
                createRender(CheckoutsTableOrigin, { origin: value }),
        }),
        table.column({
            accessor: "status",
            header: "Status",
            cell: ({ value }) =>
                createRender(CheckoutsTableStatus, { status: value }),
        }),
        table.column({
            accessor: "gateway",
            header: "Gateway",
        }),
        table.column({
            accessor: ({ charges }) => charges.length,
            header: "Cobros",
        }),
    ]);

    const { headerRows, pageRows, tableAttrs, tableBodyAttrs } =
        table.createViewModel(columns);
</script>

<div class="rounded-md border">
    <Table.Root {...$tableAttrs}>
        <Table.Header>
            {#each $headerRows as headerRow}
                <Subscribe rowAttrs={headerRow.attrs()}>
                    <Table.Row>
                        {#each headerRow.cells as cell (cell.id)}
                            <Subscribe
                                attrs={cell.attrs()}
                                props={cell.props()}
                                let:attrs
                            >
                                <Table.Head {...attrs}>
                                    <Render of={cell.render()} />
                                </Table.Head>
                            </Subscribe>
                        {/each}
                    </Table.Row>
                </Subscribe>
            {/each}
        </Table.Header>
        <Table.Body {...$tableBodyAttrs}>
            {#each $pageRows as row (row.id)}
                <Subscribe rowAttrs={row.attrs()} let:rowAttrs>
                    <Table.Row {...rowAttrs}>
                        {#each row.cells as cell (cell.id)}
                            <Subscribe attrs={cell.attrs()} let:attrs>
                                <Table.Cell {...attrs}>
                                    <Render of={cell.render()} />
                                </Table.Cell>
                            </Subscribe>
                        {/each}
                    </Table.Row>
                </Subscribe>
            {/each}
        </Table.Body>
    </Table.Root>
</div>
