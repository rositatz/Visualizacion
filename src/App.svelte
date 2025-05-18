<script>
  import * as d3 from "d3";

  let canciones = [];
  let cancionesPorDecada = {};
  let decadas = [];
  let escalaReproducciones;

  d3.csv("/Datos.csv").then(data => {
    canciones = data;

    canciones.forEach(c => {
      const decada = c.decadas;
      if (!cancionesPorDecada[decada]) {
        cancionesPorDecada[decada] = [];
        decadas.push(decada);
      }
      cancionesPorDecada[decada].push(c);
    });

    decadas.sort((a, b) => a - b);

    const maxReproducciones = d3.max(canciones, d => +d.reproducciones.replace(/[^0-9]/g, ""));
    escalaReproducciones = d3.scaleSqrt()
      .domain([0, maxReproducciones])
      .range([60, 200]);
  });

  const colorGenero = d3.scaleOrdinal()
    .domain(["Pop", "Rock", "Indie", "Electrónica", "Reguetón", "Rap"])
    .range(["#00CC66", "#CC0000", "#CC0066", "#001BCC", "#CCB400", "#000000"]);

  const simboloSelector = {
    "Vari": "src/Vari.png",
    "Rosita": "src/Rosita.png",
    "Steffy": "src/Steffy.png"
  };

  const iconosPlayPause = {
    play: "src/Vari.png",   
    pause: "src/Pause.png"  
  };

  function obtenerDiametro(reproducciones) {
    const numero = reproducciones.replace(/[^0-9]/g, "");
    return escalaReproducciones(numero);
  }
</script>
<section class="explicacion-reproductor">
  <h2> Reproductor de Canciones: Visualizar la Música</h2>
    <p>
      Cada canción se transforma en un círculo que late con su propia energía, mostrando su popularidad y estilo en un universo visual que invita a descubrir música con solo mirar.
    </p>
    
  <p>
    <strong>El tamaño</strong> del círculo representa su popularidad: cuantas más reproducciones tiene una canción, más grande se vuelve, ocupando su lugar en el escenario visual.
  </p>
  <p>
    <strong>El color</strong> refleja el género musical, pintando la visualización con una paleta tan diversa como la música misma.
  </p>
  <p>
    Si una canción llegó al <strong>Billboard</strong>, su número aparece en el centro del círculo, como un pequeño trofeo de prestigio.
  </p>
  <p>
    Cada selector tiene su símbolo: <strong>Vari</strong> es un triángulo, como el clásico botón de play <strong>Steffy</strong> un corazón, porque siente cada canción y <strong>Rosita</strong> una estrella porque brilla con su gusto musical, porque cada quien elige con su estilo.
  </p>
  <p>
    Finalmente, la <strong>danceability</strong> se traduce en la forma y movimiento, como si cada reproductor tuviera su propio pulso, latiendo al ritmo de su melodía.
  </p>
</section>


<body>
{#each decadas as decada}
  <div class="fila-con-decada">
    <div class="decada-label">{decada}</div>
    <div class="grid-canciones">
      {#each cancionesPorDecada[decada] as cancion}
        {@const diametro = obtenerDiametro(cancion.reproducciones)}
        {#key cancion.canciones}
          <div class="cancion-wrapper" style="width: {diametro + 40}px">
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
                style="
                  width: {diametro * 0.4}px;
                  height: {diametro * 0.4}px;
                "
              ></div>

              {#if +cancion.billboard !== 0}
                <div
                  class="billboard"
                  style="font-size: {diametro * 0.15}px;"
                >
                  {cancion.billboard}
                </div>
              {/if}

              <div class="iconos" style="font-size: {diametro * 0.10}px;">
                <img src={iconosPlayPause.play} alt="Play" style="width: {diametro * 0.12}px" />
                <img src={iconosPlayPause.pause} alt="Pause" style="width: {diametro * 0.12}px" />
              </div>


              <!-- Selector derecho -->
              <img
                class="selector selector-derecho"
                src={simboloSelector[cancion.eligio]}
                alt={cancion.eligio}
                style="
                  width: {diametro * 0.12}px; 
                  height: {diametro * 0.12}px;"
              />

              <!-- Selector izquierdo espejado -->
              <img
                class="selector selector-izquierdo"
                src={simboloSelector[cancion.eligio]}
                alt={cancion.eligio}
                style="
                  width: {diametro * 0.12}px;
                  height: {diametro * 0.12}px;
                "
              />
      
            </div>

            <div class="nombre-cancion">{cancion.canciones}</div>
            <div class="nombre-artista">{cancion.artistas}</div>
          </div>
        {/key}
      {/each}
    </div>
  </div>
{/each}

</body>

<style>
body {
  margin: 0;
  padding: 20px;
  background: linear-gradient(-45deg, #0d1b2a, #1b263b, #0f3460, #1b1b2f);
  background-size: 300% 300%;
  animation: fondoEstelar 30s ease infinite;
  color: white;
  font-family: 'Segoe UI', sans-serif;
}


  .fila-con-decada {
    display: flex;
    align-items: flex-start;
    margin-bottom: 40px;
  }

  .decada-label {
    font-weight: bold;
    font-size: 24px;
    width: 70px;
    margin-right: 20px;
    color: white;
  }

  .grid-canciones {
    display: grid;
    grid-template-columns: repeat(4, auto);
    gap: 20px 30px;
  }

  .cancion-wrapper {
    text-align: center;
  }

  .cancion {
    position: relative;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: auto;
  }

  .circulo-blanco {
    background-color: white;
    border-radius: 50%;
  }

  .billboard {
    position: absolute;
    top: 15%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-weight: bold;
    text-align: center;
    pointer-events: none;
  }

  .iconos {
  position: absolute;
  bottom: 15%; 
  left: 50%;
  transform: translate(-50%, 50%);
  color: white;
  text-align: center;
  pointer-events: none;
}


    .selector {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }

  .selector-derecho {
    right: 7%;
  }

  .selector-izquierdo {
    left: 7%;
  }

  .nombre-cancion {
    margin-top: 8px;
    font-size: 14px;
    color: white;
  }

  .nombre-artista {
    font-size: 12px;
    color: #aaa;
  }

  @keyframes fondoEstelar {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}


.cancion {
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.5);
}

.cancion:hover {
  filter: brightness(1.2);
  cursor: pointer;
  z-index: 10;
  transform: scale(1.1);
  transition: transform 0.3s ease, filter 0.3s ease;
}



.explicacion-reproductor {
  background: linear-gradient(to right, #091021, #0f1a2b);
  padding: 30px;
  margin: 40px auto;
  border-radius: 20px;
  width: 85%;
  color: #e0f7fa;
  font-family: 'Segoe UI', sans-serif;
  box-shadow: 0 0 20px rgba(0, 174, 255, 0.2);
}

.explicacion-reproductor h2 {
  font-size: 24px;
  margin-bottom: 15px;
  color: #a3d5ff;
  text-shadow: 0 0 5px #0055ff;
}

.explicacion-reproductor p {
  font-size: 16px;
  line-height: 1.6;
  margin-bottom: 10px;
}

</style>
