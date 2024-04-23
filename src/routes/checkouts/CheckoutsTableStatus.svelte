<script lang="ts">
    import { Badge } from "$lib/components/ui/badge";

    export let status: "charged" | "pending";

    let statusValues = {
        charged: "Cobrado",
        pending: "Pendiente",
    };

    function isCharged() {
        return status === "charged";
    }
</script>

<Badge variant={isCharged() ? "default" : "outline"}>
    {statusValues[status]}
</Badge>
