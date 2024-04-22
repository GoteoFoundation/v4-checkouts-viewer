<script lang="ts">
    import UserGlimpse from "$lib/components/content/UserGlimpse.svelte";
    import { Badge } from "$lib/components/ui/badge";

    export let status: "charged" | "pending";

    function isCharged() {
        return status === "charged";
    }
</script>

<Badge variant={isCharged() ? "default" : "outline"}>{status}</Badge>
