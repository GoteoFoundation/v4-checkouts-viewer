<script lang="ts">
    export let amount: number;
    export let currency: string;

    const formatter = new Intl.NumberFormat("es-ES", {
        style: "currency",
        currency,
    });
</script>

<span>{formatter.format(amount / 100)}</span>
