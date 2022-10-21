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
    @import "../app.scss";

    .toast {
        position: fixed;
        top: 90px;
        left: calc(50% - 140px);
        box-sizing: border-box;
        padding: $pMd;
        min-width: 280px;
        max-width: 280px;
        color: $colorTextPrimary;
        border-radius: $brMd;
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
