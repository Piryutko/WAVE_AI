<script lang="ts">
    import { ArrowRight, X } from "lucide-svelte";

    interface CarouselItem {
        id: number;
        title: string;
        label: string;
        color?: string;
        url?: string;
    }

    let {
        items = [
            { id: 1, title: "TASK_WAVE_AI", label: "AI_TASK_GENERATOR", color: "#D9C5B2", url: "https://taskwave-ai.netlify.app/" },
            { id: 2, title: "TIME_WAVE_AI", label: "AI_PLANNING_EDITOR", color: "#0070FF", url: "https://timewave-ai.netlify.app/" },
            { id: 3, title: "TONE_WAVE_AI", label: "AI_COMMUNICATION_EDITOR", color: "#10B981", url: "https://tonewave-ai.netlify.app/" },
        ],
        onSelect = (color: string) => {},
    } = $props<{ items?: CarouselItem[], onSelect?: (color: string) => void }>();

    let selectedItem = $state<CarouselItem | null>(null);

    const openModal = (item: CarouselItem) => {
        selectedItem = item;
        onSelect(item.color || "#FFFFFF");
    };

    const closeModal = () => {
        selectedItem = null;
        onSelect("#FFFFFF");
    };
</script>

<div class="carousel-container select-none">
    <div class="carousel-track">
        {#each items as item}
            <button
                class="carousel-item"
                onclick={() => openModal(item)}
                onkeydown={(e) => e.key === "Enter" && openModal(item)}
            >
                <div 
                    class="item-card"
                    style="--accent-color: {item.color ? item.color + '66' : 'rgba(255, 255, 255, 0.15)'}"
                >
                    <div class="item-content">
                        <span class="item-label">PROTOTYPE_{item.id}</span>
                        <h3 class="item-title">{item.title}</h3>
                    </div>
                    <div class="item-footer">
                        <span class="status">SYSTEM_CHECK: OK</span>
                        <ArrowRight size={14} />
                    </div>
                </div>
            </button>
        {/each}
    </div>
</div>

{#if selectedItem}
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div class="modal-backdrop" onclick={closeModal}>
        <div class="modal-content" onclick={(e) => e.stopPropagation()}>
            <button class="close-btn" onclick={closeModal}>
                <X size={20} />
            </button>
            <div class="modal-header">
                <span class="modal-label">{selectedItem.label}</span>
                {#if selectedItem.url}
                    <a href={selectedItem.url} target="_blank" class="modal-title-link">
                        <h2 class="modal-title">{selectedItem.title}</h2>
                    </a>
                {:else}
                    <h2 class="modal-title">{selectedItem.title}</h2>
                {/if}
            </div>
            <div class="modal-body">
                <!-- Content goes here -->
            </div>
        </div>
    </div>
{/if}

<style>
    .carousel-container {
        width: 100%;
        max-width: 1000px;
        height: 400px;
        display: flex;
        align-items: center;
        justify-content: center;
        overflow: visible;
        perspective: 1000px;
        z-index: 5;
    }

    .carousel-track {
        display: flex;
        gap: 2rem;
        will-change: transform;
    }

    .carousel-item {
        flex: 0 0 300px;
        height: 380px;
        transition: transform 0.5s cubic-bezier(0.23, 1, 0.32, 1);
        background: none;
        border: none;
        padding: 0;
        text-align: left;
        cursor: pointer;
    }

    .item-card {
        width: 100%;
        height: 100%;
        background: rgba(255, 255, 255, 0.02);
        backdrop-filter: blur(20px);
        -webkit-backdrop-filter: blur(20px);
        border: 1px solid rgba(255, 255, 255, 0.05);
        border-radius: 2px;
        padding: 2.5rem;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        position: relative;
        overflow: hidden;
        transition: all 0.4s ease;
    }

    .carousel-item:hover .item-card {
        background: rgba(255, 255, 255, 0.06);
        box-shadow: 0 0 35px -5px var(--accent-color);
        transform: translateY(-8px);
    }

    .item-label {
        font-family: "JetBrains Mono", monospace;
        font-size: 10px;
        letter-spacing: 0.3em;
        color: rgba(255, 255, 255, 0.3);
        display: block;
        margin-bottom: 0.5rem;
    }

    .item-title {
        font-family: "Inter", sans-serif;
        font-weight: 900;
        font-size: 1.5rem;
        letter-spacing: -0.02em;
        color: white;
        margin: 0;
    }

    .item-footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-family: "JetBrains Mono", monospace;
        font-size: 9px;
        color: rgba(255, 255, 255, 0.2);
        letter-spacing: 0.1em;
    }

    /* Modal Styles */
    .modal-backdrop {
        position: fixed;
        inset: 0;
        background: rgba(0, 0, 0, 0.8);
        backdrop-filter: blur(10px);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 100;
    }

    .modal-content {
        width: 90%;
        max-width: 600px;
        background: rgba(255, 255, 255, 0.03);
        border: 1px solid rgba(255, 255, 255, 0.1);
        padding: 3rem;
        position: relative;
        animation: modal-enter 0.4s cubic-bezier(0.23, 1, 0.32, 1);
    }

    @keyframes modal-enter {
        from {
            opacity: 0;
            transform: scale(0.95) translateY(10px);
        }
        to {
            opacity: 1;
            transform: scale(1) translateY(0);
        }
    }

    .close-btn {
        position: absolute;
        top: 1.5rem;
        right: 1.5rem;
        background: none;
        border: none;
        color: rgba(255, 255, 255, 0.3);
        cursor: pointer;
        transition: color 0.3s ease;
    }

    .close-btn:hover {
        color: white;
    }

    .modal-label {
        font-family: "JetBrains Mono", monospace;
        font-size: 10px;
        letter-spacing: 0.4em;
        color: rgba(255, 255, 255, 0.6);
        display: block;
        margin-bottom: 1rem;
    }

    .modal-title {
        font-family: "Inter", sans-serif;
        font-weight: 900;
        font-size: 2.5rem;
        letter-spacing: -0.04em;
        color: white;
        margin: 0;
        transition: color 0.3s ease;
    }

    .modal-title-link {
        text-decoration: none;
    }

    .modal-title-link:hover .modal-title {
        color: var(--accent-color, white);
        opacity: 0.8;
    }
</style>
