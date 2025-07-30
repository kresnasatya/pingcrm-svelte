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
    <div
      style="position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: black; opacity: 0.2;"
      onclick={() => (show = false)}
    ></div>
    <!-- Dropdown -->
    <div
      bind:this={dropdownEl}
      style="position: fixed;"
      onclick={(e) => { e.stopPropagation(); show = !autoclose; }}
    >
      {@render dropdown()}
    </div>
  {/if}
</div>
