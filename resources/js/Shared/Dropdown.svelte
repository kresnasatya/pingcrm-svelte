<script>
  import { createPopper } from '@popperjs/core'
  import { onDestroy, tick } from 'svelte'

  let { children, dropdown, placement = 'bottom-end', autoclose = true, ...restProps } = $props()

  let button = $state()
  let dropdownEl = $state()
  let portal = $state()
  let popper = $state()
  let show = $state(false)

  $effect.pre(async () => {
    if (show) {
      await tick()
      popper = createPopper(button, dropdownEl, {
        placement: placement,
        modifiers: [
          {
            name: 'preventOverflow',
            options: {
              altBoundary: true,
            },
          },
        ],
      })

      document.body.appendChild(portal)
    } else if (popper) {
      await tick()
      popper.destroy()
    }
  })

  function keydown(e) {
    if (e.key === 'Escape') {
      show = false
    }
  }

  onDestroy(() => {
    popper && popper.destroy()
    portal && document.body.removeChild(portal)
  })
</script>

<svelte:window onkeydown={keydown} />

<button {...restProps} bind:this={button} type="button" onclick={() => (show = true)}>
  {@render children()}
</button>

{#if show}
  <div bind:this={portal}>
    <div
      style="position: fixed; top: 0; right: 0; left: 0; bottom: 0; z-index: 99998; background:
      black; opacity: .2"
      onclick={() => (show = false)}
    ></div>
    <div bind:this={dropdownEl} style="position: absolute; z-index: 99999;" onclick={(e) => { e.stopPropagation(); show = !autoclose; }}>
      {@render dropdown()}
    </div>
  </div>
{/if}
