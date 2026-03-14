<script>
// @ts-nocheck

  import { base } from '$app/paths';
  
  // @ts-nocheck

  let isLetterOpen = $state(false);

  let scrollY = $state(0);
  let innerHeight = $state(0);
  let trackContainer;
  let trackTop = $state(0);
  let trackHeight = $state(0);

  $effect(() => {
    if (trackContainer) {
      trackTop = trackContainer.offsetTop;
      trackHeight = trackContainer.offsetHeight;
    }
  });

  let scrollProgress = $derived(
    trackHeight > 0 
      ? Math.max(0, Math.min(1, (scrollY - trackTop) / (trackHeight - innerHeight))) 
      : 0
  );

  const polaroids = [
    { id: 1, src: '/foto1.jpeg', caption: 'Nuestra segunda cita', rot: '-4deg', offX: '-10px' },
    { id: 2, src: '/foto2.jpeg', caption: 'Sonriendo contigo', rot: '3deg', offX: '15px' },
    { id: 3, src: '/foto3.jpeg', caption: 'Pijamaaas ❤️', rot: '-2deg', offX: '-20px' },
    { id: 4, src: '/foto4.jpeg', caption: 'Cita en el cine', rot: '-4deg', offX: '5px' },
    { id: 5, src: '/foto5.jpeg', caption: 'Fue divertido mientras duro', rot: '3deg', offX: '-15px' },
    { id: 6, src: '/foto6.jpeg', caption: 'Officially dating 02/14/2026 ❤️', rot: '-2deg', offX: '10px' },
    { id: 7, src: '/foto7.jpeg', caption: 'Y lo que falta ❤️', rot: '-4deg', offX: '-5px' },
  ];
</script>

<svelte:head>
  <title>Happy 1st Month, Iris! ❤️</title>
</svelte:head>

<svelte:window bind:scrollY bind:innerHeight />

<main class="mobile-container">
  <header class="hero">
    <h1>Happy first month punkiin 🐞❤️</h1>
    <p>Desliza hacia abajo despacito...</p>
  </header>

  <section class="scroll-track" bind:this={trackContainer}>
    <div class="sticky-viewport">
      <div class="gallery">
        {#each polaroids as photo, i}
          {@const start = i / polaroids.length}
          {@const end = (i + 1) / polaroids.length}
          {@const p = Math.max(0, Math.min(1, (scrollProgress - start) / (end - start)))}
          
          <div class="polaroid-piler"
               style="
                 transform: translateY({(1 - p) * -100}vh) translateX({photo.offX}) rotate({photo.rot});
                 opacity: {p > 0 ? 1 : 0};
                 z-index: {i};
               ">
            <div class="polaroid">
              <div class="image-placeholder">
                <img src="{base}{photo.src}" alt="Nosotros" onerror={(e) => e.target.style.display='none'}/>
              </div>
              <p class="caption">{photo.caption}</p>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <section class="letter-section">
    <p class="hint">Toca la carta para abrirla 💌</p>
    
    <div class="envelope" class:open={isLetterOpen} onclick={() => isLetterOpen = !isLetterOpen}>
      <div class="flap"></div>
      <div class="letter-paper">
        <h2>Para mi hermosa Iris, corazon</h2>
        <p>
          Feliz primer mes, punkiin. No puedo creer lo rápido que ha pasado este tiempo. 
          Me siento el hombre más afortunado del mundo por tenerte a mi lado. Gracias por 
          cada sonrisa y cada momento especial. Este es solo el primero de muchos meses juntos.
        </p>
        <p class="signature">Con todo amor,<br>Luz</p>
      </div>
      <div class="envelope-front"></div>
    </div>
  </section>
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    background-color: #fdf2f8; 
    font-family: 'Helvetica Neue', sans-serif;
    overflow-x: hidden; 
  }

  .mobile-container {
    max-width: 430px; 
    margin: 0 auto;
    background: #fdf2f8;
    padding-bottom: 100px;
    box-shadow: 0 0 20px rgba(0,0,0,0.05);
  }

  .hero {
    text-align: center;
    padding: 60px 20px;
    height: 100vh; 
    display: flex;
    flex-direction: column;
    justify-content: center;
    box-sizing: border-box;
  }

  h1 {
    color: #db2777;
    font-size: 2.2rem;
    line-height: 1.2;
    margin-bottom: 10px;
  }
  
  .scroll-track {
    height: 600vh; 
    position: relative;
  }

  .sticky-viewport {
    position: sticky;
    top: 0;
    height: 100vh; 
    display: flex;
    align-items: center; 
    justify-content: center; 
  }

  .gallery {
    display: grid;
    place-items: center;
    width: 100%;
  }

  .polaroid-piler {
    grid-area: 1 / 1;
    width: 280px; 
    will-change: transform; 
  }

  .polaroid {
    background: white;
    padding: 12px 12px 40px 12px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.15);
    border-radius: 4px;
    border: 1px solid #f1f5f9;
  }

  .image-placeholder {
    width: 100%;
    aspect-ratio: 1/1;
    background: #e2e8f0;
    overflow: hidden;
  }

  .image-placeholder img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .caption {
    text-align: center;
    margin-top: 15px;
    font-size: 1.2rem;
    color: #333;
    font-family: 'Comic Sans MS', cursive, sans-serif;
  }

  /* --- Animación de la Carta Corregida --- */
  .letter-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    margin-top: 50px; 
    min-height: 100vh;
    justify-content: center;
  }

  .hint {
    color: #888;
    margin-bottom: 20px;
    font-size: 0.9rem;
  }

  .envelope {
    position: relative;
    width: 300px;
    height: 200px;
    background: #f472b6; /* Fondo del interior del sobre */
    cursor: pointer;
    perspective: 1000px;
  }

  /* El frente del sobre ahora tiene bordes en 3 lados para cubrir por completo la carta */
  .envelope-front {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-left: 150px solid #ec4899;
    border-right: 150px solid #ec4899;
    border-bottom: 100px solid #db2777;
    border-top: 100px solid transparent;
    box-sizing: border-box;
    z-index: 3;
    pointer-events: none;
  }

  .flap {
    position: absolute;
    top: 0;
    left: 0;
    width: 0;
    height: 0;
    border-left: 150px solid transparent;
    border-right: 150px solid transparent;
    border-top: 120px solid #db2777;
    z-index: 4;
    transform-origin: top;
    transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .letter-paper {
    position: absolute;
    top: 10px;
    left: 10px;
    right: 10px;
    max-height: 180px; /* Límite para que quepa en el sobre cerrado */
    overflow: hidden; /* Oculta el texto que sobra */
    background: white;
    padding: 20px;
    box-sizing: border-box;
    border-radius: 4px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    z-index: 2;
    /* Animamos el transform y la altura máxima al mismo tiempo */
    transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1), max-height 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .letter-paper h2 {
    font-size: 1rem;
    color: #db2777;
    margin-top: 0;
  }

  .letter-paper p {
    font-size: 0.85rem;
    line-height: 1.5;
    color: #444;
  }

  .signature {
    text-align: right;
    font-weight: bold;
    margin-top: 20px;
  }

  /* Clases activas al abrir la carta */
  .envelope.open .flap {
    transform: rotateX(180deg);
    z-index: 1;
  }

  .envelope.open .letter-paper {
    transform: translateY(-200px); /* Sube la carta */
    max-height: 500px; /* Expande la carta para mostrar todo el mensaje */
    z-index: 5;
  }
</style>