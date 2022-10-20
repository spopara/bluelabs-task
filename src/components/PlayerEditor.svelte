<script lang="ts">
    import PlayerForm from "../components/PlayerForm.svelte"
    import type { Player } from "../interfaces"
    import Button from "./Button.svelte"

    export let show = false
    export let onClose: () => void
    export let submitPlayer: (p: Player) => void
    export let player: Partial<Player>

    let editingPlayer: Player | undefined
</script>

<dialog class="player-editor" aria-modal="true" open="{show}" data-backdrop="static" data-keyboard="false">
    <h2 id="form-dialog-title">
        {player.name ? `Edit ${player.name}` : "New player"}
    </h2>
    <div class="player-editor-form">
        <PlayerForm
            id="{player.id}"
            name="{player.name}"
            position="{player.position}"
            picture="{player.picture}"
            score="{player.score}"
            goals="{player.goals}"
            onUpdate="{(data) => (editingPlayer = data)}"
        />
    </div>

    <div>
        <Button
            on:click="{() => {
                submitPlayer(editingPlayer)
            }}"
        >
            Submit
        </Button>

        <Button
            on:click="{(e) => {
                e.preventDefault()
                onClose()
            }}"
        >
            Close
        </Button>
    </div>
</dialog>

<style lang="scss">
    .player-editor {
        position: absolute;
        top: 100px;
        left: 0;
        right: 0;
        background-color: #fff;
        border-radius: 25px;
        text-align: center;

        .player-editor-form {
            position: relative;
            box-sizing: border-box;
            padding: 10px;
            margin: auto;
            max-width: 512px;
        }
    }
</style>
