<script>
    export let decada;
    export let canciones;
    export let colorGenero;
    export let obtenerDiametro;
    export let iconosPlayPause;
    export let simboloSelector;
    

    
    import { onMount } from 'svelte';
    
    // Step 1: Update the wave position to be more visible (moved up)
    // Step 2: Use danceability to control wave amplitude
   
    onMount(() => {
  canciones.forEach(cancion => {
    const waveContainers = document.querySelectorAll(`[data-cancion="${cancion.canciones.replace(/"/g, '\\"')}"] .wave-container`);

    waveContainers.forEach(container => {
      const svg = container.querySelector('svg');
      const path = container.querySelector('path');
      const genre = cancion.generos;
      const color = colorGenero(genre);
      path.setAttribute('stroke', color);

      // Get the circle diameter from style
      const circleWrapper = container.closest('.cancion');
      const diametroPx = circleWrapper ? parseFloat(window.getComputedStyle(circleWrapper).width) : 100;

      // Stroke width scaling
      const minStroke = 0.5;
      const maxStroke = 3;
      const maxDiametro = 150;
      const clampedDiametro = Math.min(diametroPx, maxDiametro);
      const strokeWidth = minStroke + (maxStroke - minStroke) * (clampedDiametro / maxDiametro);
      path.setAttribute('stroke-width', strokeWidth);

      // Danceability
      const danceability = parseFloat(cancion.danceability || 50);
      const maxAmplitude = 6;
      const baseAmplitude = 1;
      const amplitude = baseAmplitude + (maxAmplitude - baseAmplitude) * (danceability / 100);

      // Frequency based on danceability
      const minFrequency = 0.0;  // Super low for low-danceability songs
    const maxFrequency = 0.65;   // Higher than before for high-danceability songs
        const frequency = minFrequency + (maxFrequency - minFrequency) * (danceability / 100);

      const baseY = parseInt(svg.getAttribute('height')) * 0.16;
      const width = parseInt(svg.getAttribute('width'));

      let d = `M 0 ${baseY}`;
      for (let x = 0; x < width; x += 1) {
        const waveY = Math.sin(x * frequency);
        const y = baseY + waveY * amplitude;
        d += ` L ${x} ${y}`;
      }

      path.setAttribute('d', d);
    });
  });
});



</script>

   
   
   <div class="fila-con-decada">
     <div class="decada-label">{decada}</div>
     <div class="grid-canciones" style="grid-template-rows: repeat({canciones.length > 4 ? 2 : 1}, 1fr);">
       {#each canciones as cancion (cancion.canciones)}
         {@const diametro = obtenerDiametro(cancion.reproducciones) * 0.88}
         <div class="cancion-wrapper" data-cancion="{cancion.canciones}">
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
             >
               <!-- Wave animation container -->
               <div class="wave-container" style="width: 100%; height: 100%; position: relative;">
                 <svg width="100%" height="100%" viewBox="0 0 100 30" preserveAspectRatio="none">
                   <path 
                     d="" 
                     fill="none" 
                     stroke={colorGenero(cancion.generos)} 
                     stroke-width="3"
                     stroke-linecap="round"
                     vector-effect="non-scaling-stroke"
                   />
                 </svg>
               </div>
             </div>
             
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
   
   <style>
     /* Add these styles to your component */
     .wave-container {
       display: flex;
       align-items: center;
       justify-content: center;
       overflow: hidden;
       border-radius: 50%;
       position: absolute;
       top: 0;
       left: 0;
       width: 100%;
       height: 100%;
     }
     
     .circulo-blanco {
       display: flex;
       align-items: center;
       justify-content: center;
       position: relative;
       border-radius: 50%;
       background-color: white;
       z-index: 2;
       overflow: hidden;
     }
   </style>
   
   