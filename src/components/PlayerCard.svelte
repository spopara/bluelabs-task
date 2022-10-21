<script lang="ts">
    import { createEventDispatcher } from "svelte"
    import { fade, scale } from "svelte/transition"
    import type { Player } from "../interfaces"
    import Button from "./Button.svelte"

    export let player: Player

    const dispatchEvent = createEventDispatcher()

    const deletePlayer = (id: string) => {
        dispatchEvent("delete-player", id)
    }

    const selectPlayer = (player: Player) => {
        dispatchEvent("edit-player", player)
    }
</script>

<li class="player-card" out:fade in:scale>
    <div class="player-card-info">
        <p class="player-card-title" title="{player.name}">{player.name}</p>
        <p class="player-card-subtitle">{player.position}</p>

        <dl class="player-card-statistics">
            <dt class="player-card-statistics-item player-card-statistics-item--title">Score</dt>
            <dd class="player-card-statistics-item player-card-statistics-item--value">
                {player.score}
            </dd>
            <dt class="player-card-statistics-item player-card-statistics-item--title">Goals</dt>
            <dd class="player-card-statistics-item player-card-statistics-item--value">
                {player.goals}
            </dd>
        </dl>
    </div>

    <img
        class="player-card-image"
        src="{player.picture}"
        alt="{`of ${player.name}`}"
        title="Image {`of ${player.name}`}"
    />

    <div class="player-card-controls">
        <Button on:click="{() => deletePlayer(player.id)}">Delete</Button>
        <Button aria-controls="player-editor" on:click="{() => selectPlayer(player)}">Edit</Button>
    </div>
</li>

<style lang="scss">
    $lightGray: #d4d4d4;
    $cardMargin: 10px;

    .player-card {
        position: relative;
        box-sizing: border-box;
        margin: $cardMargin;
        width: 335px;
        height: 500px;
        flex-shrink: 0;
        color: #fff;
        background: #2b6298;
        background: radial-gradient(circle, #2b6298 0%, #123053 100%);
        border-radius: 25px;
        overflow: hidden;

        .player-card-info {
            box-sizing: border-box;
            padding: 20px;
            height: 206px;

            .player-card-title {
                margin: 0 auto;
                font-size: 2rem;
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;
            }

            .player-card-subtitle {
                margin: 0 auto $cardMargin;
                color: $lightGray;
                font-size: 1rem;
            }

            .player-card-statistics {
                margin-bottom: $cardMargin;
                display: grid;
                grid-template-columns: repeat(2, 50%);
                grid-template-rows: repeat(2, 50%);
                grid-auto-flow: column;

                .player-card-statistics-item {
                    margin: 0;
                    justify-self: center;
                }

                .player-card-statistics-item--title {
                    font-size: 1.2em;
                }

                .player-card-statistics-item--value {
                    font-size: 1.8em;
                    font-weight: bold;
                }
            }
        }

        .player-card-image {
            width: 100%;
            height: 294px;
            object-fit: cover;
            object-position: 50% 0;
        }

        .player-card-controls {
            position: absolute;
            bottom: 0;
            box-sizing: border-box;
            padding: 10px 20px;
            width: 100%;
            display: flex;
            justify-content: space-around;
            align-items: center;
        }
    }
</style>
