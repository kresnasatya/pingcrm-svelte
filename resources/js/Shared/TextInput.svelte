<script>
  import { nanoid } from 'nanoid'
  import Label from '@/Shared/Label.svelte'

  let {
    id = `text-input-${nanoid(5)}`,
    class: className,
    value = $bindable(),
    label,
    type = 'text',
    error
  } = $props()

  let input

  export const focus = () => input.focus()
  export const select = () => input.select()

  function update(event) {
    value = event.target.value
  }
</script>

<div class={className}>
  <Label {label} {id} />

  <input bind:this={input} class="form-input" class:error {id} {type} bind:value oninput={update} />

  {#if error}
    <div class="form-error">{error}</div>
  {/if}
</div>
