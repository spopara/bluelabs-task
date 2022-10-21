<script lang="ts">
    import { onMount } from "svelte"
    import { fade } from "svelte/transition"
    import type { ToastType } from "../interfaces"

    export let message = ""
    export let type: ToastType = "info"
    export let close: () => void
    const timeout = 3000

    onMount(() => {
        setTimeout(() => {
            close && close()
        }, timeout)
    })
</script>

<div
    class="toast"
    class:toast--info="{type === 'info'}"
    class:toast--success="{type === 'success'}"
    class:toast--error="{type === 'error'}"
    out:fade
    aria-live="polite"
>
    {message}
</div>

<style lang="scss">
    $colorInfo: #898989;
    $colorSuccess: #75c371;
    $colorError: #b85151;
    .toast {
        position: fixed;
        top: 90px;
        left: calc(50% - 150px);
        box-sizing: border-box;
        padding: 10px;
        min-width: 300px;
        max-width: 300px;
        color: #fff;
        border-radius: 15px;
        text-align: center;

        &--info {
            background-color: $colorInfo;
            border: 2px solid darken($colorInfo, 25%);
        }

        &--success {
            background-color: $colorSuccess;
            border: 2px solid darken($colorSuccess, 25%);
        }

        &--error {
            background-color: $colorError;
            border: 2px solid darken($colorError, 25%);
        }
    }
</style>
