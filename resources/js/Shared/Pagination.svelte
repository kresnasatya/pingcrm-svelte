<script>
  import { inertia } from '@inertiajs/svelte'

  let { links = [], ...rest } = $props();
</script>

{#if links.length > 3}
  <div class={`-mb-1 flex flex-wrap ${rest.class || ''}`}>
    {#each links as link, key (key)}
      {#if link.url === null}
        <div class="mb-1 mr-1 rounded-sm border px-4 py-3 text-sm leading-4 text-gray-400">
          {@html link.label}
        </div>
      {:else}
        <a use:inertia href={link.url} class={`mb-1 mr-1 rounded-sm border px-4 py-3 text-sm leading-4 hover:bg-white focus:border-indigo-500 focus:text-indigo-500 ${link.active && 'bg-white'}`}>
          {@html link.label}
        </a>
      {/if}
    {/each}
  </div>
{/if}
