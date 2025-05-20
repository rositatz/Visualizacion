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

<head>
  <!-- Importa la fuente Raleway desde Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Raleway&display=swap" rel="stylesheet" />
</head>

<style>
  .generos-circulos {
    display: flex;
    justify-content: center;
    gap: 40px;
    flex-wrap: wrap;
    margin-top: 20px;
    margin-bottom: 40px;
  }

  .genero {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Raleway', sans-serif;
    font-size: 14px;
  }

  .circulo {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    margin-bottom: 8px;
  }
</style>

<div class="explicacion-reproductor">
  <h2>Cuando el ritmo se vuelve interfaz</h2>
  <p>
    Un recorrido visual por las décadas musicales, donde cada canción se transforma en diseño, y cada reproductor refleja nuestra forma de escuchar, elegir... y sentir la música.
    Desde los vinilos que giraban con un susurro nostálgico, pasando por los casetes que se enrollaban como recuerdos atrapados, hasta los reproductores digitales que hoy habitan en la palma de nuestra mano, la música ha sido siempre más que sonido: es experiencia, memoria y evolución. Este viaje no solo muestra la estética de cada época, sino también cómo la tecnología ha moldeado nuestra conexión con las melodías que nos acompañan. Prepárate para descubrir cómo el ritmo se vuelve interfaz y la música, un paisaje para explorar con todos los sentidos.   
  </p>
  <br>
  <br>
  <h3>Un cosmos musical donde cada círculo cuenta su historia.</h3>
</div>

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

  <p>
    Cada canción se transforma en un círculo que late con su propia energía, mostrando su popularidad y estilo en un universo visual que invita a descubrir música con solo mirar.

    <br>
    <strong>El tamaño</strong> del círculo representa su popularidad: cuantas más reproducciones tiene una canción, más grande se vuelve, ocupando su lugar en el escenario visual.
    <br>
    <strong>El color</strong> refleja el género musical, pintando la visualización con una paleta tan diversa como la música misma.
  </p>

  <!-- Círculos de colores de los géneros -->
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

  <p>
    Si una canción llegó al <strong>Billboard</strong>, su número aparece en el centro del círculo, como un pequeño trofeo de prestigio.
    <br>
    Cada selector tiene su símbolo: <strong>Var</strong> es un triángulo, como el clásico botón de play <strong>Steffy</strong> un corazón, porque siente cada canción y <strong>Rosita</strong> una estrella porque brilla con su gusto musical, porque cada quien elige con su estilo.
    <br>
    Finalmente, la <strong>danceability</strong> se traduce en la forma y movimiento, como si cada reproductor tuviera su propio pulso, latiendo al ritmo de su melodía.
    <br>
    En el centro de cada canción, <strong>una onda estática</strong> muestra el ritmo de la música, adoptando el color de su género musical. La amplitud de la onda refleja la danceability de la canción: cuanto más bailable, más pronunciada será la onda.
  </p>
</body>
