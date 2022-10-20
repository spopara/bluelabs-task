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
        players = players.filter((player) => player.id !== id)
    }

    const updatePlayer = (player: Player) => {
        selectedPlayer = undefined
        players = players.map((p) => (p.id === player.id ? player : p))
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
        <Button id="add-player" on:click="{() => (isEditing = true)}">
            <span>Add</span>
        </Button>
    </menu>

    <ul>
        {#each players as player (player.id)}
            <PlayerCard
                player="{player}"
                on:delete-player="{(e) => deletePlayer(e.detail)}"
                on:edit-player="{(e) => {
                    selectedPlayer = e.detail
                    isEditing = true
                }}"
            />
        {/each}
    </ul>
</section>

<PlayerEditor
    show="{isEditing || !!selectedPlayer}"
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
