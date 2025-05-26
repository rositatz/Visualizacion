<script>
  import * as d3 from "d3";
  import CancionesPorDecada from "./components/CancionesPorDecada.svelte";
  import Footer from "./components/Footer.svelte";
  
  let canciones = [];
  let cancionesPorDecada = {};
  let decadas = [];
  let escalaReproducciones;
  let estadosReproduccion = {};

  
  d3.csv("/Datos.csv").then(data => {
    //test
    canciones = data;
    canciones.forEach(c => {
      const decada = c.decadas;
      if (!cancionesPorDecada[decada]) {
        cancionesPorDecada[decada] = [];
        decadas.push(decada);
      }
      cancionesPorDecada[decada].push(c);
    });

// Ordenar las canciones dentro de cada década por número de reproducciones (de menor a mayor)
Object.keys(cancionesPorDecada).forEach(decada => {
  cancionesPorDecada[decada].sort((a, b) => {
    const repA = +a.reproducciones.replace(/[^0-9]/g, "");
    const repB = +b.reproducciones.replace(/[^0-9]/g, "");
    return repA - repB; 
  });
});

    const ordenPersonalizado = ["20", "10", "00", "90", "80"];
    decadas.sort((a, b) => ordenPersonalizado.indexOf(a) - ordenPersonalizado.indexOf(b));
    
    const maxReproducciones = d3.max(canciones, d => +d.reproducciones.replace(/[^0-9]/g, ""));
    escalaReproducciones = d3.scaleSqrt()
      .domain([0, maxReproducciones])
      .range([60, 200]);
  });
  
  const colorGenero = d3.scaleOrdinal()
    .domain(["Pop", "Rock", "Indie", "Electrónica", "Reguetón", "Rap"])
    .range(["#00CC66", "#CC0000", "#CC0066", "#001BCC", "#CCB400", "#000000"]);
  
  const simboloSelector = {
    "Var": "/images/Var.png",
    "Rosita": "/images/Rosita.png",
    "Steffy": "/images/Steffy.png"
  };
  
  const iconosPlayPause = {
    play: "/images/Var.png",
    pause: "/images/Pause.png"
  };
  
  function obtenerDiametro(reproducciones) {
    const numero = reproducciones.replace(/[^0-9]/g, "");
    return escalaReproducciones(numero);
  }
</script>

<head>
  <!-- Importa la fuente Raleway desde Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Raleway&display=swap" rel="stylesheet" />
</head>

<body>
  <div class="explicacion-reproductor">
    <h2 id="titulo-musical">
      Notas Compartidas
      <br>
      que Suenan en N osotras
    </h2>
    <p>
      <script>
        function reemplazarOsConSimbolos() {
          const titulo = document.getElementById('titulo-musical');
          const texto = titulo.innerHTML;
    
          const simbolos = ['❤️', '⭐', '▶'];
          let contador = 0;
    
          const nuevoTexto = texto.replace(/o/gi, () => {
            const simbolo = simbolos[contador % simbolos.length];
            contador++;
            return `<span class="simbolo-musical">${simbolo}</span>`;
          });
    
          titulo.innerHTML = nuevoTexto;
        }
    
        reemplazarOsConSimbolos();
      </script>
      Desde los éxitos virales de 2020 hasta los clásicos inolvidables de los 80, este recorrido revela cómo la música ha transformado nuestra forma de sentir, bailar y escuchar a lo largo del tiempo.
      A través de canciones representativas de cada década, se analizan datos como la cantidad de reproducciones, su posición en el 
      Billboard Top 100, el género musical y el nivel de danceability. 
      <br>
      Este viaje musical no solo celebra el ritmo y la melodía, sino también cómo cada época dejó su huella en nuestras playlists, conectando emociones, historias y tendencias con un solo click.
      <br>
      Porque la música no solo se escucha: se siente, se vive y se recuerda.
    </p>
    <br>
    <br>
    <h3 class="titulo-reproducir">Modo Reproducir Nosotras</h3>
  </div>
    
  
  <div class="codificaciones">
    <!-- Círculos de colores de los géneros -->
    <div class="fila-codificacion fila-superior">
      <div class="codificacion-wrapper">
        <div class="fila-codificacion">
          <div class="item-codificacion">
            <div class="titulo-codificacion">Quién eligió la canción</div>
            <div style="display: flex; justify-content: center; align-items: center;gap: 25px; margin-bottom: 10px;">
                <div style="text-align: center;">
                  <img src="/images/Play.png" alt ="Var" style="height: 25px;" /><br>Var
                </div>
                <div style="text-align: center;">
                  <img src="/images/Heart.png" alt ="Steffy" style="height: 25px;" /><br>Steffy
                </div>
                <div style="text-align: center;">
                  <img src="/images/Star.png" alt ="Rosita" style="height: 25px;" /><br>Rosita
                </div>
              </div>
          </div>
        </div>
      </div>
      <div class="item-codificacion">
        <div class="titulo-codificacion">Danceability</div>
        <div class="ondas-danceability">
          <svg viewBox="0 0 100 40" class="onda onda-baja">
            <path d="M 0 20 C 10 10, 20 30, 30 20 C 40 10, 50 30, 60 20 C 70 10, 80 30, 90 20, 100 20" />
          </svg>

          <svg viewBox="0 0 100 40" class="onda onda-media">
            <path d="M 0 20 C 10 0, 20 40, 30 20 C 40 0, 50 40, 60 20 C 70 0, 80 40, 90 20, 100 20" />
          </svg>

          <svg viewBox="0 0 100 40" class="onda onda-alta">
            <path d="M 0 20 C 10 -10, 20 50, 30 20 C 40 -10, 50 50, 60 20 C 70 -10, 80 50, 90 20, 100 20" />
          </svg>
        </div>
    </div>
  </div>
        <h3 class="titulo-codificacion">Género</h3>
        <div class="generos-circulos">
          <div class="genero">
            <div class="circulo" style="background-color: #00CC66;"></div>
            <span>Pop</span>
          </div>
          <div class="genero">
            <div class="circulo" style="background-color: #CC0000;"></div>
            <span>Rock</span>
          </div>
          <div class="genero">
            <div class="circulo" style="background-color: #CC0066;"></div>
            <span>Indie</span>
          </div>
          <div class="genero">
            <div class="circulo" style="background-color: #001BCC;"></div>
            <span>Electrónica</span>
          </div>
          <div class="genero">
            <div class="circulo" style="background-color: #CCB400;"></div>
            <span>Reguetón</span>
          </div>
          <div class="genero">
            <div class="circulo" style="background-color: #000000;"></div>
            <span >Rap</span>
          </div>
        </div>
        <div class="fila-codificacion">
      <div class="codificacion-wrapper">
        <h3 class="titulo-codificacion">Popularidad</h3>
        <div class="popularidad-circulos">
          <div class="circulo pequeno"></div>
          <div class="circulo mediano"></div>
          <div class="circulo grande"></div>
        </div>
      </div>

      <div class="codificacion-wrapper billboard-wrapper">
        <h3 class="titulo-codificacion">Ranking Billboard</h3>
        <div class="billboard-numero">77</div>
      </div>
      </div>
      </div>

      
 

    {#each decadas as decada}
      <CancionesPorDecada
        {decada}
        canciones={cancionesPorDecada[decada]}
        {colorGenero}
        {obtenerDiametro}
        {iconosPlayPause}
        {simboloSelector}
      />
    {/each}

    <p>
      Cada canción se transforma en un círculo que late con su propia energía, mostrando su popularidad y estilo en un universo visual que invita a descubrir música con solo mirar.
      <br>
      <br>
      <strong>El tamaño</strong> del círculo representa su popularidad: cuantas más reproducciones tiene una canción, más grande se vuelve.
      <br>
      <strong>El color</strong> refleja el género musical, pintando la visualización con una paleta tan diversa como la música misma.
      <br>
      Si una canción llegó al <strong>Billboard</strong>, su número aparece en la parte superior del círculo, como un pequeño trofeo de prestigio.
      <br>
      <br>
      Cada selector tiene su símbolo: <strong>Var</strong> es un triángulo, como el clásico botón de play, <strong>Steffy</strong> un corazón, porque siente cada canción y <strong>Rosita</strong> una estrella porque brilla con su gusto musical, cada quien elige con su estilo.
      <br>
      <br>
      Finalmente, la <strong>danceability</strong> se traduce en la forma y movimiento, como si cada reproductor tuviera su propio pulso, latiendo al ritmo de su melodía.
      <br>
      En el centro de cada canción, <strong>una onda estática</strong> muestra el ritmo de la música, adoptando el color de su género musical. La amplitud de la onda refleja la danceability de la canción: cuanto más bailable, más pronunciada será la onda.
    </p>
    <Footer/>
</body>
