<script>
  import { nanoid } from 'nanoid'
  import Label from '@/Shared/Label.svelte'

  let {
    id = `file-input-${nanoid(5)}`,
    value = $bindable(),
    label,
    accept,
    errors = [],
    ...restProps
  } = $props()

  let files = $state()
  let input

  export const browse = () => input.click()

  const inputProps = $derived({
    ...restProps,
    class: 'form-input p-0',
  })

  const error = $derived(errors !== undefined && errors.length > 0)

  $effect(() => {
    value = files ? files[0] : null
  })

  function filesize(size) {
    var i = Math.floor(Math.log(size) / Math.log(1024))
    return (size / Math.pow(1024, i)).toFixed(2) * 1 + ' ' + ['B', 'kB', 'MB', 'GB', 'TB'][i]
  }

  function remove() {
    files = null
    // Clear the input value to allow re-uploading the same file
    if (input) {
      input.value = ''
    }
  }
</script>

<div class={restProps.class}>
  <Label {label} {id} />

  <div {...inputProps} class:error>
    <input bind:this={input} bind:files class="hidden" type="file" {id} {accept} />

    {#if !value}
      <div class="p-2">
        <button type="button" class="x-4 rounded-xs bg-gray-500 py-1 text-xs font-medium text-white hover:bg-gray-700" onclick={browse}> Browse </button>
      </div>
    {:else}
      <div class="flex items-center justify-between p-2">
        <div class="flex-1 pr-1">
          {value.name}
          <span class="text-xs text-gray-500">({filesize(value.size)})</span>
        </div>
        <button type="button" class="rounded-xs bg-gray-500 px-4 py-1 text-xs font-medium text-white hover:bg-gray-700" onclick={remove}> Remove </button>
      </div>
    {/if}
  </div>

  {#if error}
    <div class="form-error">{errors[0]}</div>
  {/if}
</div>
