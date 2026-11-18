<script lang="ts">
    let { data } = $props();
    import LoggedInHome from '../lib/components/loggedinhome.svelte';
    import Home from '../lib/components/home.svelte';
</script>

{#if data.user}
    <LoggedInHome />
{:else}
    <Home />
{/if}

