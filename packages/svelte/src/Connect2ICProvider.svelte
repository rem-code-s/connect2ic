<script lang="ts">
  import type { RootContext, RootEvent } from "@connect2ic/core"
  import { contextKey, type ContextState } from "./context"
  import { setContext } from "svelte"
  import { readable, writable } from "svelte/store"

  export let client
  let open = writable(false)

  const dialog = {
    open: () => {
      open.set(true)
    },
    close: () => {
      open.set(false)
    },
    isOpen: open,
  }

  let action = writable<{
    type: string
    context: RootContext
    event: RootEvent
  }>(undefined)

  setContext<ContextState>(contextKey, {
    client,
    dialog,
  })
</script>

<slot />
