<script>
    import { onMount } from "svelte";
  
    let artistId = "";
    let artistData = null;
  
    const searchArtistById = async () => {
      if (artistId.trim() !== "") {
        try {
          const response = await fetch(`http://localhost:8000/artistas/${artistId}`);
          const responseData = await response.json();
  
          // Trate diferentes formatos de resposta
          if (responseData.name !== undefined) {
            // Se a resposta tem um formato esperado
            artistData = {
              nome: responseData.name,
              biografia: responseData.biography,
              imagem: responseData.imagem
            };
          } else {
            console.error("Formato inesperado dos dados do artista:", responseData);
          }
        } catch (error) {
          console.error("Erro ao buscar o artista:", error);
        }
      }
    };
  
    onMount(() => {
      // Chame a função de busca apenas no lado do cliente
      searchArtistById();
    });
  </script>
  
  <div>
    <label for="artistId">ID do Artista:</label>
    <input type="text" id="artistId" bind:value={artistId} />
  
    <button on:click={searchArtistById}>Buscar Artista por ID</button>
  
    {#if artistData}
      <div>
        <h3>Informações do Artista:</h3>
        <p>Nome: {artistData.nome}</p>
        <p>Biografia: {artistData.biografia}</p>
        <img src={artistData.imagem} alt={`Imagem de ${artistData.nome}`} />
      </div>
    {/if}
  </div>
  