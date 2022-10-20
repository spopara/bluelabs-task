<script lang="ts">
    import { nanoid } from "nanoid"
    import PlayerEditor from "../components/PlayerEditor.svelte"
    import type { Player, ToastType } from "../interfaces"
    import { toPosition } from "../utils"
    import Button from "./Button.svelte"
    import PlayerCard from "./PlayerCard.svelte"

    export let players: Array<Player> = []
    export let showToast: (message: string, type: ToastType) => void
    let selectedPlayer: Player | undefined
    let showEditor = false

    const deletePlayer = (id: string) => {
        fetch("/players", {
            method: "DELETE",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ id }),
        })
            .then((response) => {
                if (response.ok) return response
                else throw new Error("Could not delete player.")
            })
            .then(() => {
                players = players.filter((p) => p.id !== id)
                handleServerSuccess("Player deleted successfully.")
            })
            .catch((error) => handleServerError(error))
    }

    const updatePlayer = (player: Player) => {
        closeEditor()
        fetch("/players", {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(player),
        })
            .then((response) => {
                if (response.ok) return response
                else throw new Error("Could not update player.")
            })
            .then(() => {
                players = players.map((p) => (p.id === player.id ? player : p))
                handleServerSuccess("Player updated successfully.")
            })
            .catch((error) => handleServerError(error))
    }

    const addPlayer = (player: Player) => {
        closeEditor()
        fetch("/players", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(player),
        })
            .then((response) => {
                if (response.ok) return response
                else throw new Error("Could not add player.")
            })
            .then(() => {
                players = [...players, player]
                handleServerSuccess("Player added successfully.")
            })
            .catch((error) => handleServerError(error))
    }

    const handleServerSuccess = (message: string): void => {
        showToast(message, "success")
    }

    const handleServerError = (error: string): void => {
        console.error(error)
        showToast(error, "error")
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
        <Button on:click="{() => (showEditor = true)}">
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

{#if showEditor}
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
{/if}

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
