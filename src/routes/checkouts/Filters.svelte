<script lang="ts">
    import * as Card from "$lib/components/ui/card";
    import * as Select from "$lib/components/ui/select";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    let filters: string[][] = [];

    function handleStatus(e: any) {
        filters = [...filters, ["status", e.value]];

        if (e.value === "all") {
            filters = filters.filter((f) => f[0] !== "status");
        }

        dispatch("change", { value: filters });
    }
</script>

<Card.Root>
    <Card.Header>
        <Card.Title>Filtros</Card.Title>
        <Card.Description>
            {filters.length} filtros aplicados
        </Card.Description>
    </Card.Header>
    <Card.Content class="pt-1 h-80 overflow-y-auto">
        <Select.Root onSelectedChange={handleStatus}>
            <Select.Trigger class="mb-5">
                <Select.Value placeholder="Estado" />
            </Select.Trigger>
            <Select.Content>
                <Select.Item value="all">Todos</Select.Item>
                <Select.Item value="charged">Cobrados</Select.Item>
                <Select.Item value="pending">Pendientes de cobro</Select.Item>
            </Select.Content>
        </Select.Root>
    </Card.Content>
</Card.Root>
