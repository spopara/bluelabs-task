<script lang="ts">
    import { nanoid } from "nanoid"
    import PlayerEditor from "../components/PlayerEditor.svelte"
    import type { Player } from "../interfaces"
    import { toPosition } from "../utils"
    import Button from "./Button.svelte"
    import PlayerCard from "./PlayerCard.svelte"

    export let players: Array<Player> = []
    let selectedPlayer: Player | undefined
    let isEditing = false

    const deletePlayer = (id: string) => {
        // TODO: add `DELETE` api request (endpoint: `/players`, accepted payload: player id)
        throw new Error("Delete request not implemented")
    }

    const updatePlayer = (player: Player) => {
        // TODO: add `PUT` api request (endpoint: `/players`, accepted payload: player)
        throw new Error("Put request not implemented")
    }

    const addPlayer = (player: Player) => {
        // TODO: add `POST` api request (endpoint: `/players`, accepted payload: player)
        throw new Error("Post request not implemented")
    }

    const getDefaultPlayer = () => {
        return {
            id: nanoid(),
            position: toPosition("Goalkeeper"),
            score: 0,
            goals: 0,
        }
    }
</script>

<section>
    <menu>
        <Button id="add-player" on:click="{() => (isEditing = !isEditing)}">
            <span>Add</span>
        </Button>
    </menu>

    <ul>
        {#each players as player (player.id)}
            <PlayerCard
                player="{player}"
                on:delete-player="{(e) => deletePlayer(e.detail)}"
                on:select-player="{(e) => (selectedPlayer = e.detail)}"
            />
        {/each}
    </ul>
</section>

<!-- {#if isEditing || !!selectedPlayer} -->
<PlayerEditor
    show={isEditing || !!selectedPlayer}
    onClose="{() => {
        isEditing = false
        selectedPlayer = undefined
    }}"
    submitPlayer="{(player) => {
        if (selectedPlayer) {
            return updatePlayer(player)
        } else {
            return addPlayer(player)
        }
    }}"
    player="{selectedPlayer || getDefaultPlayer()}"
/>

<!-- {/if} -->
<style lang="scss">
    menu {
        padding: 10px;
        display: flex;
        justify-content: end;
        align-items: center;
    }
    section {
        width: 100%;
        ul {
            padding: 0;
            margin: 0 0 20px 0;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
            align-items: center;
            list-style: none;
        }
    }
</style>
