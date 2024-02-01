<script lang="ts">
    import Drawer from "./Drawer.svelte";
    export let name: string;
</script>

<Drawer>
    <svelte:fragment slot="trigger">
        <!-- Referring to the named containers -->
        <p class="@sm/row:underline">{name}</p>
    </svelte:fragment>

    <svelte:fragment slot="content">
        <p>Content whatever here to show</p>
    </svelte:fragment>
</Drawer>