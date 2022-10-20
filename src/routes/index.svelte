<script lang="ts">
    import { onMount } from "svelte"
    import MainContent from "../components/MainContent.svelte"
    import Toast from "../components/Toast.svelte"
    import type { Player, ToastType } from "../interfaces"

    let loading = true
    let toastMessage = ""
    let toastType: ToastType = "info"
    let players: Array<Player> = []

    const hideToast = (): void => {
        toastMessage = ""
        toastType = "info"
    }

    const showToast = (message: string, type: ToastType): void => {
        toastMessage = message
        toastType = type
    }

    onMount(() => {
        // TODO:
        // - Notify the user of any errors
        // - This part can also be (optionally) optimised to retrieve the players
        //   server-side and prerender the page
        fetch("/players")
            .then((response) => {
                if (response.ok) return response.json()
                else throw new Error("Could not retrieve players")
            })
            .then((data) => {
                players = data
                loading = false
            })
            .catch((error) => {
                console.error(error)
                showToast(error, "error")
            })
    })
</script>

{#if loading}
    <h2>Loading...</h2>
{:else}
    <MainContent players="{players}" showToast="{showToast}" />
{/if}

{#if toastMessage}
    <Toast message="{toastMessage}" type="{toastType}" close="{hideToast}" />
{/if}
