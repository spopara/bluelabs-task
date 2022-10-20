<script>
    export let type = "button"
</script>

<button on:click type="{type}">
    <slot />
</button>

<style lang="scss">
    button {
        padding: 8px 12px;
        background-color: #ffcc74;
        color: #000;
        font-weight: bold;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }

    button:hover {
        color: #fff;
    }
</style>
