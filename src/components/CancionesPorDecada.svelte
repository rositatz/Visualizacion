<script>
    export let decada;
    export let canciones;
    export let colorGenero;
    export let obtenerDiametro;
    export let iconosPlayPause;
    export let simboloSelector;
  </script>
  
  <div class="fila-con-decada">
    <div class="decada-label">{decada}</div>
    <div class="grid-canciones" style="grid-template-rows: repeat({canciones.length > 4 ? 2 : 1}, 1fr);">
        {#each canciones as cancion (cancion.canciones)}
        {@const diametro = obtenerDiametro(cancion.reproducciones) * 0.88}

        <div class="cancion-wrapper">
          <div
            class="cancion"
            style="
              width: {diametro}px;
              height: {diametro}px;
              background-color: {colorGenero(cancion.generos)};
              font-size: {diametro * 0.1}px;
            "
          >
            <div
              class="circulo-blanco"
              style="width: {diametro * 0.4}px; height: {diametro * 0.4}px;"
            ></div>
      
            {#if +cancion.billboard !== 0}
              <div class="billboard" style="font-size: {diametro * 0.15}px;">
                {cancion.billboard}
              </div>
            {/if}
      
            <div class="iconos" style="font-size: {diametro * 0.10}px;">
              <img src={iconosPlayPause.play} alt="Play" style="width: {diametro * 0.12}px" />
              <img src={iconosPlayPause.pause} alt="Pause" style="width: {diametro * 0.12}px" />
            </div>
      
            <img
              class="selector selector-derecho"
              src={simboloSelector[cancion.eligio]}
              alt={cancion.eligio}
              style="width: {diametro * 0.12}px; height: {diametro * 0.12}px;"
            />
            <img
              class="selector selector-izquierdo"
              src={simboloSelector[cancion.eligio]}
              alt={cancion.eligio}
              style="width: {diametro * 0.12}px; height: {diametro * 0.12}px;"
            />
          </div>
      
          <div class="nombre-cancion">{cancion.canciones}</div>
          <div class="nombre-artista">{cancion.artistas}</div>
        </div>
      {/each}
      
    </div>
  </div>
  