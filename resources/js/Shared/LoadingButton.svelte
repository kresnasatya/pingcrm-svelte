<!-- @migration-task Error while migrating Svelte code: migrating this component would require adding a `$props` rune but there's already a variable named props.
     Rename the variable and try again or migrate by hand. -->
<!-- @migration-task Error while migrating Svelte code: migrating this component would require adding a `$props` rune but there's already a variable named props.
     Rename the variable and try again or migrate by hand. -->
<!-- @migration-task Error while migrating Svelte code: migrating this component would require adding a `$props` rune but there's already a variable named props.
     Rename the variable and try again or migrate by hand. -->
<script>
  export let loading = false

  $: props = {
    ...$$restProps,
    class: `flex items-center ${$$restProps.class || ''}`,
  }
</script>

<button {...props} disabled={loading}>
  {#if loading}
    <div class="btn-spinner mr-2"></div>
  {/if}

  <slot />
</button>
