 <script>
    import Personagem from '$lib/components/Personagem.svelte';
    import { personagens } from '$lib/cards.js';
</script>

<div class="row g-4">
    {#each personagens as personagem}
        <div class="col">
            <Personagem {...personagem} />
        </div>
    {/each}
</div>
