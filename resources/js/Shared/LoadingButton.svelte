<script>
  let { children, loading = $bindable(false), class: className, ...props } = $props()
</script>

<button class="flex items-center {className}" {...props} disabled={loading}>
  {#if loading}
    <div class="btn-spinner mr-2"></div>
  {/if}

  {@render children()}
</button>
