<script lang="ts">
    import * as Card from "$lib/components/ui/card";
    import * as Select from "$lib/components/ui/select";
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    let filters: string[][] = [];

    function handleStatus(e: any) {
        filters = [
            ...filters.filter((f: string[]) => f[0] !== "status[]"),
            ...e.map((f: any) => ["status[]", f.value]),
        ];

        dispatch("change", { value: filters });
    }

    function handleGateway(e: any) {
        filters = [
            ...filters.filter((f: string[]) => f[0] !== "gateway[]"),
            ...e.map((f: any) => ["gateway[]", f.value]),
        ];

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
        <Select.Root multiple onSelectedChange={handleStatus}>
            <Select.Trigger class="mb-5">
                <Select.Value placeholder="Estado" />
            </Select.Trigger>
            <Select.Content>
                <Select.Item value="charged">Cobrados</Select.Item>
                <Select.Item value="pending">Pendientes de cobro</Select.Item>
            </Select.Content>
        </Select.Root>
        <Select.Root multiple onSelectedChange={handleGateway}>
            <Select.Trigger class="mb-5">
                <Select.Value placeholder="Pasarela de pago" />
            </Select.Trigger>
            <Select.Content>
                <Select.Item value="cash">Pago en efectivo</Select.Item>
                <Select.Item value="paypal">Paypal</Select.Item>
                <Select.Item value="wallet">Monedero virtual</Select.Item>
                <Select.Item value="stripe">Stripe</Select.Item>
                <Select.Item value="tpv">TPV</Select.Item>
            </Select.Content>
        </Select.Root>
    </Card.Content>
</Card.Root>
