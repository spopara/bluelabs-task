<script lang="ts">
    import PlayerForm from "../components/PlayerForm.svelte"
    import type { Player } from "../interfaces"
    import Button from "./Button.svelte"

    export let onClose: () => void
    export let submitPlayer: (p: Player) => void
    export let player: Partial<Player>
    let editingPlayer: Player = player as Player
</script>

<dialog
    id="player-editor"
    class="player-editor"
    aria-modal="true"
    data-backdrop="static"
    data-keyboard="false"
    aria-label="Player Editor"
>
    <h2 id="form-dialog-title">
        {player.name ? `Edit ${player.name}` : "New player"}
    </h2>
    <form
        class="player-editor-form"
        on:submit|preventDefault="{() => {
            submitPlayer(editingPlayer)
        }}"
    >
        <PlayerForm
            id="{player.id}"
            name="{player.name}"
            position="{player.position}"
            picture="{player.picture}"
            score="{player.score}"
            goals="{player.goals}"
            onUpdate="{(data) => (editingPlayer = data)}"
        />

        <div>
            <Button type="submit">Submit</Button>

            <Button
                on:click="{() => {
                    onClose()
                }}"
            >
                Close
            </Button>
        </div>
    </form>
</dialog>

<style lang="scss">
    .player-editor {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        background-color: #fff;
        border-radius: 25px;
        text-align: center;

        &::backdrop {
            background: #2b6298;
            background: radial-gradient(circle, #2b6298 0%, #123053 100%);
            pointer-events: none;
        }

        .player-editor-form {
            position: relative;
            box-sizing: border-box;
            padding: 10px;
            margin: auto;
            max-width: 512px;
        }
    }
</style>
