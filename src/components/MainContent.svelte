<script lang="ts">
    import { nanoid } from "nanoid"
    import { onMount } from "svelte"
    import PlayerEditor from "../components/PlayerEditor.svelte"
    import type { Player, ToastType } from "../interfaces"
    import { toPosition } from "../utils"
    import Button from "./Button.svelte"
    import PlayerCard from "./PlayerCard.svelte"

    export let players: Array<Player> = []
    export let showToast: (message: string, type: ToastType) => void
    let addDialog: HTMLDialogElement = undefined
    let addOrUpdatePlayer: (player: Player) => void = undefined
    const getDefaultPlayer = (): Player => {
        return {
            id: nanoid(),
            name: "",
            position: toPosition("Goalkeeper"),
            score: 0,
            goals: 0,
            picture: "",
        }
    }
    let selectedPlayer: Player = getDefaultPlayer()

    onMount(() => {
        addDialog = document.querySelector("#player-editor")
    })

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
        handleCloseEditor()
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
        handleCloseEditor()
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

    const handleAddPlayer = () => {
        addOrUpdatePlayer = addPlayer
        selectedPlayer = getDefaultPlayer()
        addDialog?.showModal()
    }

    const handleEditPlayer = (player: Player) => {
        addOrUpdatePlayer = updatePlayer
        selectedPlayer = player
        addDialog?.showModal()
    }

    const handleCloseEditor = () => {
        addDialog?.close()
        selectedPlayer = getDefaultPlayer()
    }
</script>

<section>
    <menu>
        <Button on:click="{() => handleAddPlayer()}">Add</Button>
    </menu>

    <ul>
        {#each players as player (player.id)}
            <PlayerCard
                player="{player}"
                on:delete-player="{(e) => deletePlayer(e.detail)}"
                on:edit-player="{(e) => handleEditPlayer(e.detail)}"
            />
        {/each}
    </ul>
</section>

<PlayerEditor
    onClose="{() => {
        handleCloseEditor()
    }}"
    submitPlayer="{addOrUpdatePlayer}"
    player="{selectedPlayer}"
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
