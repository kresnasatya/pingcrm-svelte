<script>
  import Dropdown from '@/Shared/Dropdown.svelte'

  let { children, onreset, value = $bindable(), maxWidth = 300, class: className, ...restProps } = $props()
</script>

<div class="flex items-center {className}" {...restProps}>
  <div class="flex w-full rounded-sm bg-white shadow-sm">
    <Dropdown class="h-full rounded-l border-r px-4 hover:bg-gray-100 focus:z-10 focus:border-white focus:ring-3 md:px-6" placement="bottom-start" autoclose={false}>
      <div class="flex items-baseline">
        <span class="hidden text-gray-700 md:inline">Filter</span>
        <svg class="h-2 w-2 fill-gray-700 md:ml-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 961.243 599.998">
          <path
            d="M239.998 239.999L0 0h961.243L721.246 240c-131.999 132-240.28 240-240.624
            239.999-.345-.001-108.625-108.001-240.624-240z"
          />
        </svg>
      </div>
      {#snippet dropdown()}
        <div class="mt-2 w-screen rounded-sm bg-white px-4 py-6 shadow-xl" style="max-width: {maxWidth}px">
          {@render children()}
        </div>
      {/snippet}
    </Dropdown>

    <input class="focus:shadow-outline relative w-full rounded-r px-6 py-3" autocomplete="off" type="text" name="search" placeholder="Search…" bind:value />
  </div>

  <button class="ml-3 text-sm text-gray-500 hover:text-gray-700 focus:text-indigo-500" type="button" onclick={() => onreset?.()}>Reset</button>
</div>
