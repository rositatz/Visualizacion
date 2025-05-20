<script>
  import * as d3 from "d3";
  import CancionesPorDecada from "./components/CancionesPorDecada.svelte";
  
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
  <h2>Reproductor de Canciones: Visualizar la Música</h2>
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
    Cada selector tiene su símbolo: <strong>Var</strong> es un triángulo, como el clásico botón de play <strong>Steffy</strong> un corazón, porque siente cada canción y <strong>Rosita</strong> una estrella porque brilla con su gusto musical, porque cada quien elige con su estilo.
  </p>
  <p>
    Finalmente, la <strong>danceability</strong> se traduce en la forma y movimiento, como si cada reproductor tuviera su propio pulso, latiendo al ritmo de su melodía.
  </p>
  <p>
    En el centro de cada canción, <strong>una onda estática</strong> muestra el ritmo de la música, adoptando el color de su género musical. La amplitud de la onda refleja la danceability de la canción: cuanto más bailable, más pronunciada será la onda.
  </p>
 </section>
 
 <body>
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


</body>


