<script>
  import { nanoid } from 'nanoid'
  import Label from '@/Shared/Label.svelte'

  let {
    children,
    id = `select-input-${nanoid(5)}`,
    value = $bindable(),
    label,
    error,
    onchange,
    ...restProps
  } = $props()

  let input

  export const focus = () => input.focus()

  let selectProps = $derived({
    ...restProps,
    class: 'form-select',
  })

  function update(event) {
    event.preventDefault()
    value = typeof value === 'number' ? parseInt(event.target.value) : event.target.value
  }
</script>

<div class={restProps.class}>
  <Label {label} {id} />

  <select {...selectProps} bind:this={input} class:error {id} bind:value onblur={update}>
    {@render children()}
  </select>

  {#if error}
    <div class="form-error">{error}</div>
  {/if}
</div>
