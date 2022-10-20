<script lang="ts">
    import { nanoid } from "nanoid"
    import PlayerEditor from "../components/PlayerEditor.svelte"
    import type { Player } from "../interfaces"
    import { toPosition } from "../utils"
    import Button from "./Button.svelte"
    import PlayerCard from "./PlayerCard.svelte"

    export let players: Array<Player> = []
    let selectedPlayer: Player | undefined
    let showEditor = false

    const deletePlayer = (id: string) => {
        players = players.filter((player) => player.id !== id)
    }

    const updatePlayer = (player: Player) => {
        closeEditor()
        players = players.map((p) => (p.id === player.id ? player : p))
    }

    const addPlayer = (player: Player) => {
        closeEditor()
        players = [player, ...players]
    }

    const closeEditor = () => {
        showEditor = false
        selectedPlayer = undefined
    }

    const getDefaultPlayer = () => {
        return {
            id: nanoid(),
            name: "",
            position: toPosition("Goalkeeper"),
            score: 0,
            goals: 0,
        }
    }
</script>

<section>
    <menu>
        <Button id="add-player" on:click="{() => (showEditor = true)}">
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
                    showEditor = true
                }}"
            />
        {/each}
    </ul>
</section>

<PlayerEditor
    show="{showEditor}"
    onClose="{closeEditor}"
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
