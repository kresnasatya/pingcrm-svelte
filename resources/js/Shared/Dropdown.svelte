<!-- Dropdown.svelte -->
<script>
  import { computePosition, flip, shift, offset, autoUpdate } from '@floating-ui/dom'
  import { onDestroy, tick } from 'svelte'

  let { children, dropdown, placement = 'bottom-end', autoclose = true, ...restProps } = $props()

  let button = $state()
  let dropdownEl = $state()
  let show = $state(false)
  let cleanup = $state()

  async function updatePosition() {
    if (!button || !dropdownEl) return

    const { x, y } = await computePosition(button, dropdownEl, {
      placement: placement,
      middleware: [
        offset(4),
        flip(),
        shift({ padding: 8 })
      ]
    })

    dropdownEl.style.left = `${x}px`
    dropdownEl.style.top = `${y}px`
  }

  $effect.pre(async () => {
    if (show && button && dropdownEl) {
      await tick()

      // Initial positioning
      await updatePosition()

      // Auto-update on scroll, resize, etc.
      cleanup = autoUpdate(button, dropdownEl, updatePosition)
    } else if (cleanup) {
      cleanup()
      cleanup = null
    }
  })

  function keydown(e) {
    if (e.key === 'Escape') {
      show = false
    }
  }

  onDestroy(() => {
    cleanup?.()
  })
</script>

<svelte:window onkeydown={keydown} />

<div style="position: relative; display: inline-block;">
  <button {...restProps} bind:this={button} type="button" onclick={() => (show = true)}>
    {@render children()}
  </button>

  {#if show}
    <!-- Backdrop -->
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <div
      style="position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: black; opacity: 0.2; z-index: 1;"
      onclick={() => (show = false)}
      aria-label="Close dropdown"
      role="button"
      tabindex="-1"
    ></div>
    <!-- Dropdown -->
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div
      bind:this={dropdownEl}
      style="position: fixed; z-index: 2;"
      onclick={(e) => { e.stopPropagation(); show = !autoclose; }}
    >
      {@render dropdown()}
    </div>
  {/if}
</div>
