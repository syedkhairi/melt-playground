<script lang="ts">
    import { createDialog, melt } from '@melt-ui/svelte';
    import { fade, fly } from 'svelte/transition';
   
    const {
        elements: {
            trigger,
            overlay,
            content,
            title,
            description,
            close,
            portalled,
        },
        states: { open },
    } = createDialog({
        forceVisible: true,
    });
</script>
   
<button use:melt={$trigger}>
    <slot name="trigger" />
</button>

  <div class="" use:melt={$portalled}>
    {#if $open}
        <div
            use:melt={$overlay}
            class="fixed inset-0 z-50 bg-black/50"
            transition:fade={{ duration: 150 }}
        />
        <div
            use:melt={$content}
            class="fixed left-0 top-0 z-50 h-screen w-full max-w-[350px] bg-white p-6
                shadow-lg focus:outline-none"
            transition:fly={{
                x: -350,
                duration: 300,
                opacity: 1,
            }}
        >
            <slot name="content" />
        </div>
    {/if}
</div>