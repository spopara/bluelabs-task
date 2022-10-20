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
>
    {message}
</div>

<style lang="scss">
    .toast {
        position: fixed;
        top: 90px;
        left: calc(50% - 150px);
        padding: 10px;
        min-width: 300px;
        color: #fff;
        border-radius: 15px;
        text-align: center;

        &--info {
            background-color: #898989;
            border: 2px solid darken(#898989, 25%);
        }

        &--success {
            background-color: #75c371;
            border: 2px solid darken(#75c371, 25%);
        }

        &--error {
            background-color: #b85151;
            border: 2px solid darken(#b85151, 25%);
        }
    }
</style>
