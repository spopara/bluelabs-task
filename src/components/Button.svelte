<button {...$$restProps} on:click>
    <slot />
</button>

<style lang="scss">
    @import "../app.scss";
    button {
        padding: $pSm $pMd;
        background-color: $colorSecondary;
        color: $colorTextSecondary;
        font-weight: bold;
        border: none;
        border-radius: $brSm;
        cursor: pointer;
    }

    button:hover {
        color: $colorTextPrimary;
    }
</style>
