<script lang="ts">
  import { useConnect, useDialog, useProviders } from "../stores/index"
  import { onMount } from "svelte"

  export let onClose: () => void = () => {
    close()
  }
  export let dark: boolean = false

  const onClickInside = (e) => {
    e.stopPropagation()
  }

  const { isOpen, close, open } = useDialog()
  const providers = useProviders()
  const { connect } = useConnect({
    onConnect: () => {
      close()
    },
  })

  if ($isOpen) {
    document.body.style.overflow = "hidden"
  }
  if (!$isOpen) {
    document.body.style.overflow = "unset"
  }

  onMount(() => {
    const handleEsc = (event) => {
      if (event.keyCode === 27) {
        close()
      }
    }
    window.addEventListener("keydown", handleEsc)
    return () => {
      window.removeEventListener("keydown", handleEsc)
    }
  })
</script>

{#if $isOpen}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div class={`dialog-styles ${dark ? "dark" : "light"}`} on:click={onClose}>
    <div on:click={onClickInside} class="dialog-container">
      <div>
        {#each $providers as provider}
          <button
            on:click={() => connect(provider.meta.id)}
            class={`button-styles ${provider.meta.id}-styles`}
          >
            <!-- svelte-ignore a11y-missing-attribute -->
            <img
              class={"img-styles"}
              src={dark ? provider.meta.icon.dark : provider.meta.icon.light}
            />
            <div>
              <span class="button-label">{provider.meta.name}</span>
            </div>
          </button>
        {/each}
      </div>
    </div>
  </div>
{/if}
