<script lang="ts">
  import { useConnect, useDialog } from "../stores/index"

  export const onConnect: () => void = () => {}
  export const onDisconnect: () => void = () => {}
  export const dark: boolean = false
  export const style: string = ""

  const isICX = !!(window as any).icx
  const { open } = useDialog()
  const { isConnected, disconnect, connect } = useConnect({
    onConnect,
    onDisconnect,
  })
</script>

{#if $isConnected}
  <button {style} class="connect-button" on:click={() => disconnect()}>
    Disconnect
  </button>
{/if}

{#if !$isConnected}
  <button
    {style}
    class="connect-button"
    on:click={() => (isICX ? connect("icx") : open())}
  >
    Connect
  </button>
{/if}
