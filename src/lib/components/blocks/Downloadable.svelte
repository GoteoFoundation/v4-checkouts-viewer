<script lang="ts">
    import Button from "../ui/button/button.svelte";

    export let url: string;
    export let options: RequestInit = { method: "GET" };

    function getFilenameValidDateString(): string {
        const date = new Date();
        const dateYmd = date.toISOString().split("T")[0];
        const dateHis = `${date.getHours()}${date.getMinutes()}${date.getSeconds()}`;

        return `${dateYmd}_${dateHis}`;
    }

    function fetchDownload() {
        fetch(url, options)
            .then((res) => res.blob())
            .then((res) => {
                const href = URL.createObjectURL(res);

                const path = new URL(url).pathname.split("/").pop() || "";
                const date = getFilenameValidDateString();

                const aElement = document.createElement("a");
                aElement.href = href;
                aElement.setAttribute("download", `${path}_${date}`);
                aElement.setAttribute("target", "_blank");
                aElement.click();

                URL.revokeObjectURL(href);
            });
    }
</script>

<Button on:click={fetchDownload} on:keydown={fetchDownload}>
    <slot />
</Button>
