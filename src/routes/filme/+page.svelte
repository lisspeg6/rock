<script>
    import { filmes, gêneros } from '$lib/filmes.js';
  
    let gênerosSelecionados = $state([]);
    let filtrados = $state(filmes.slice());
    let searchQuery = $state('');
  
   
    function filtrarGenero(event) {
        if (event.target.checked) {
            gênerosSelecionados.push(event.target.value);
        } else {
            gênerosSelecionados.splice(gênerosSelecionados.indexOf(event.target.value), 1);
        }
  
        if (gênerosSelecionados.length == 0 && searchQuery === '') {
            filtrados = filmes.slice();
        } else {
            filtrados = filmes.filter(filme => {
                
                const matchesGenre = gênerosSelecionados.length === 0 || filme.gêneros.some(gênero => gênerosSelecionados.includes(gênero));
                
                const matchesSearch = filme.título.toLowerCase().includes(searchQuery.toLowerCase());
  
                return matchesGenre && matchesSearch;
            });
        }
    }
  
   
    function atualizarBusca(event) {
        searchQuery = event.target.value;
        filtrarGenero(event); 
    }
  </script>
  
  <div class="row align-items-center mb-3">
    <div class="col">
      <input
        class="form-control"
        placeholder="Filtrar..."
        bind:value={searchQuery}
        oninput={atualizarBusca} />
    </div>
    {#each gêneros as gênero}
        <div class="col">
            <div class="form-check form-check-inline">
                <input oninput={filtrarGenero} class="form-check-input" type="checkbox" id={gênero} value={gênero} />
                <label class="form-check-label" for={gênero}>{gênero}</label>
            </div>
        </div>
    {/each}
  </div>
  
  <div class="row g-4">
    {#each filtrados as filme}
        <div class="col-md-6 col-xl-3">
            <div class="card h-100">
                <div class="row g-0">
                    <div class="col-3 col-sm-4">
                        <img src={filme.imagem} class="img-fluid rounded-start" alt="capa do filme" />
                    </div>
                    <div class="col-sm-8">
                        <div class="card-body">
                            <h5 class="card-title">{filme.título}</h5>
                            <h6 class="card-subtitle mb-2 text-body-secondary">{filme.ano}</h6>
                            <p class="card-text">{filme.sinopse}</p>
                            <p class="card-text">
                                {#each filme.gêneros as gênero}
                                    <span class="badge text-bg-secondary mx-1">{gênero}</span>
                                {/each} <p>
                            <a href={filme.referência} target="_blank" class="btn btn-primary">Ver no IMDb</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    {/each}
  </div>
  