<script>
  import { onMount, onDestroy } from "svelte";
  import { Map } from "mapbox-gl";
  import Scrolly from "./Scrolly.svelte";
  import { tweened } from "svelte/motion";
  import Scroll from "./Scrolly.svelte";
  import "../node_modules/mapbox-gl/dist/mapbox-gl.css";

  let data = [
    {
      KM: "",
      Ciudad: "",
      País: "",
      x: 0,
      y: 10,
      zoom: 1.5,
    },
    {
      KM: "Km 0",
      Ciudad: "Buenos Aires",
      País: "Argentina",
      y: -34.6037,
      x: -58.3816,
      image: "./img/img_satellite/Buenos_Aires.png",
    },
    {
      KM: "Km 13",
      Ciudad: "Villa Martelli",
      País: "Argentina",
      y: -34.5553,
      x: -58.4849,
    },
    {
      KM: "Km 15",
      Ciudad: "Sáenz Peña",
      País: "Argentina",
      y: -34.6098,
      x: -58.5734,
    },
    {
      KM: "Km 17.8",
      Ciudad: "Ciudadela",
      País: "Argentina",
      y: -34.6372,
      x: -58.5295,
      image: "./img/img_satellite/Buenos_Aires.png",
    },
    {
      KM: "Km 18",
      Ciudad: "Caseros",
      País: "Argentina",
      y: -34.6091,
      x: -58.5638,
    },
    {
      KM: "Km 18.2",
      Ciudad: "San Isidro",
      País: "Argentina",
      y: -34.4746,
      x: -58.5389,
    },
    {
      KM: "Km 20",
      Ciudad: "Béccar",
      País: "Argentina",
      y: -34.4585,
      x: -58.5565,
      image: "./img/img_satellite/Buenos_Aires.png",
    },
    {
      KM: "Km 31.1",
      Ciudad: "Tigre",
      País: "Argentina",
      y: -34.4251,
      x: -58.5796,
      image: "./img/img_satellite/Buenos_Aires.png",
    },
    {
      KM: "Km 52",
      Ciudad: "La Plata",
      País: "Argentina",
      y: -34.9205,
      x: -57.9536,
    },
    {
      KM: "Km 59",
      Ciudad: "Cañuelas",
      País: "Argentina",
      y: -35.0515,
      x: -58.7602,
    },
    {
      KM: "Km 70",
      Ciudad: "Luján",
      País: "Argentina",
      y: -34.5703,
      x: -59.105,
    },
  ];

  let map;
  let stepElements = [];
  let mapContainer;
  let mapLoaded;
  let lng, lat, zoom;
  let userInteracting = false;
  let spinEnabled = true;
  let click = false;
  let pointPositions = [];

  let imageUrl = "./img/logo-bienal-white.svg"; // change this to your image path
  // At low zooms, complete a revolution every two minutes.
  const secondsPerRevolution = 120;
  // Above zoom level 5, do not rotate.
  const maxSpinZoom = 5;
  // Rotate at intermediate speeds between zoom levels 3 and 5.
  const slowSpinZoom = 3;
  let filledLineLength = 0;
  const kmValues = data.map((d) => parseFloat(d.KM.split(" ")[1] || 0));
  let initialStateKM = kmValues;

  function handleScroll() {
    const scrollPercentage =
      (window.scrollY / (document.body.scrollHeight - window.innerHeight)) *
      100;
    filledLineLength = scrollPercentage;
  }

  function calculatePointPositions() {
    // Créer une copie de data sans le premier élément
    let points = data.slice(1);

    let totalPoints = data.length;
    console.log(points);
    pointPositions = data.map((_, index) => (index / totalPoints) * 100);
    console.log(pointPositions);
  }

  function random(min, max) {
    return Math.random() * (max - min) + min;
  }

  function spinGlobe() {
    const zoom = map.getZoom();
    if (spinEnabled && !userInteracting && zoom < maxSpinZoom) {
      let distancePerSecond = 360 / secondsPerRevolution;
      if (zoom > slowSpinZoom) {
        // Slow spinning at higher zooms
        const zoomDif = (maxSpinZoom - zoom) / (maxSpinZoom - slowSpinZoom);
        distancePerSecond *= zoomDif;
      }
      const center = map.getCenter();
      center.lng -= distancePerSecond;
      // Smoothly animate the map over one second.
      // When this animation is complete, it calls a 'moveend' event.
      map.easeTo({ center, duration: 1000, easing: (n) => n });
    }
  }

  const setFlyTo = function (data, currentStep) {
    if (currentStep == 0) {
      data = {
        bearing: 0,
        center: [data.x, data.y],
        zoom: 1.5,
        pitch: 0,
      };
      map.flyTo(data);
      spinGlobe();
    } else {
      data = {
        bearing: random(0, 90),
        center: [data.x, data.y],
        zoom: random(12, 14),
        pitch: random(30, 45),
      };
      map.flyTo(data);
    }
  };

  let currentStep;
  let steps = data.map((item, index) => {
    if (item.image) {
      console.log(item);
      return {
        content: `<img src="${item.image}" alt="Image pour l'étape ${index}" class="responsive-image">`,
      };
    } else {
      return {
        content: ``,
      };
    }
  });

  // steps = ["<p>Tableau 0!</p>", "<p>Tableau 1?</p>", "<p>Tableau 2.</p>"];

  $: if (currentStep != -1 && currentStep < data.length) {
    console.log(currentStep);
    setFlyTo(data[currentStep], currentStep);
  }

  // Fonction pour déclencher le défilement
  function scrollToCurrentStep() {
    const currentElement = stepElements[currentStep];
    if (currentElement) {
      currentElement.scrollIntoView({ behavior: "smooth" });
    }
  }

  function handlePointClick(index) {
    currentStep = index;
    console.log("Point clicked, current step:", currentStep);
    scrollToCurrentStep();
  }

  onMount(() => {
    window.scrollTo(0, 0);

    // Réinitialiser currentStep à 0
    currentStep = -1;

    calculatePointPositions();
    window.addEventListener("scroll", handleScroll);

    if (currentStep >= 0) {
      scrollToCurrentStep();
    }

    function spinGlobe() {
      const zoom = map.getZoom();
      if (spinEnabled && !userInteracting && zoom < maxSpinZoom) {
        let distancePerSecond = 360 / secondsPerRevolution;
        if (zoom > slowSpinZoom) {
          // Slow spinning at higher zooms
          const zoomDif = (maxSpinZoom - zoom) / (maxSpinZoom - slowSpinZoom);
          distancePerSecond *= zoomDif;
        }
        const center = map.getCenter();
        center.lng -= distancePerSecond;
        // Smoothly animate the map over one second.
        // When this animation is complete, it calls a 'moveend' event.
        map.easeTo({ center, duration: 1000, easing: (n) => n });
      }
    }

    const initialState = { lng: lng, lat: lat, zoom: zoom };
    const initialStateKM = { km: kmValues };

    map = new Map({
      container: mapContainer,
      accessToken:
        "pk.eyJ1Ijoic29nZWZpY2FtZXJvdW4iLCJhIjoiY2xnemF3d2dzMGhtZTNocWNweHMxM2UwYiJ9.jjR5E8aTDTeOg70oRZ9d1A",
      // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
      style: "mapbox://styles/sogeficameroun/clnn9xr3i008m01qp4h430o5e",
      zoom: 1.5,
      center: [0, 10],
      interactive: false,
      pitch: 0,
      projection: "globe",
    });

    map.loadImage("./img/google-maps.png", function (error, image) {
      if (error) throw error;
      map.addImage("VOTRE_ICONE", image);
    });

    map.on("load", function () {
      map.setFog({});

      map.addSource("points", {
        type: "geojson",
        data: "./data/point_bienal.geojson",
      });

      map.addLayer({
        id: "points",
        type: "symbol",
        source: "points",
        minzoom: 8,
        layout: {
          "icon-image": "VOTRE_ICONE",
          "icon-size": 0.25,
        },
      });
      mapLoaded = true;
    });
    map.once("load", function () {
      let userInteracting = false;
      let spinEnabled = true;
      spinGlobe();
    });
    // When animation is complete, start spinning if there is no ongoing interaction
    map.on("moveend", () => {
      spinGlobe();
    });
    let click = true;
  });
  onDestroy(() => {
    map.remove();
    window.removeEventListener("scroll", handleScroll);
  });
</script>

<div>
  <div id="features" />

  <div class="map-wrap">
    <div class="map" bind:this={mapContainer} />
  </div>
  <div class="center-container">
    <!-- Un conteneur intermédiaire pour la ligne, qui sera centré verticalement -->
    <div class="line-container">
      {#each data as point, i}
        {#if point.Ciudad != ""}
          <div
            class="point"
            style="left: {pointPositions[i]}%"
            on:click={() => handlePointClick(i)}
          >
            <div
              class={currentStep === i
                ? "point-circle point-circle-active"
                : "point-circle"}
            />
            <span class="point-text">{point.Ciudad}</span>
          </div>
        {/if}
      {/each}
      <div class="line" />
    </div>
    <div class="filled-line" style="width: {filledLineLength}%" />
  </div>
  {#if currentStep == 0}
    <img src={imageUrl} alt="Logo" class="logo-image" />
  {/if}

  {#if currentStep >= 1}
    <div class="sidebar">
      <div class="left-content">
        <div>Km: {initialStateKM[currentStep]}</div>
      </div>
      <div class="right-content">
        {data[currentStep].Ciudad} | {data[currentStep].País}
      </div>
    </div>
  {/if}
  <!-- Le conteneur principal qui centre la ligne sur la page -->
  <Scroll bind:value={currentStep}>
    {#each steps as step, i}
      <div
        class="step"
        class:active={currentStep === i}
        bind:this={stepElements[i]}
      >
        {#if currentStep >= 1}
          <div class="step-content">
            {@html step.content}
            <!-- Utilisation dynamique de HTML -->
          </div>
        {/if}
      </div>
    {/each}
  </Scroll>
</div>

<style>
  .map-wrap {
    position: sticky;
    top: 0; /* La carte restera collée en haut */
    width: auto;
    height: 90vh;
    z-index: 10;
  }
  .map {
    position: absolute;
    width: 100%;
    height: 100%;
  }

  .left-content,
  .right-content {
    display: flex;
    font-size: 2.5em;
    align-items: center; /* Si vous voulez aligner les textes verticalement */
  }

  .sidebar {
    background-color: black;
    color: #fff;
    padding: 6px 12px;
    font-family: monospace;
    z-index: 11;
    position: sticky;
    top: 0;
    left: 0;
    display: flex;
    justify-content: space-between; /* Ceci sépare les enfants (items) aux deux extrémités du conteneur */
    align-items: center; /* Ceci les aligne verticalement */
    padding: 10px; /* Ajoutez un peu d'espace des deux côtés, ajustez comme nécessaire */
    /* margin: 12px; */
    border-radius: 4px;
    font-family: "TabletGothicWideRegular", sans-serif;
  }

  #features {
    /* position: relative; */
    width: 100%;
    margin-left: 0%;
    font-family: sans-serif;
    overflow-y: scroll;
    background-color: rgba(0, 0, 0, 0);
  }

  #stickyNumber {
    position: fixed;
    top: 10%;
    left: 10px;
    font-size: 64px;
    z-index: 1000;
    color: white;
  }

  /* Scrollytelling CSS */
  .step {
    height: 400vh;
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .step-content {
    /* background: whitesmoke;
    color: #ccc; */
    border-radius: 5px;
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    transition: background 500ms ease;
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
    z-index: 10;
    width: 25%;
    position: absolute;
  }

  .step.active .step-content {
    /* background: white; */
    color: black;
  }

  /* Line and dots*/
  /* Style pour le conteneur qui va centrer la ligne et les points */
  .center-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 10vh;
    width: 100%;
    z-index: 13;
    position: fixed;
    background-color: #000;
  }

  .line-container {
    display: flex;
    justify-content: space-around;
    align-items: center;
    position: relative;
    height: 100px;
    width: 100%;
  }
  .line {
    position: absolute;
    width: 100%;
    height: 2px; /* Épaisseur de la ligne */
    background-color: black;
    top: 50%;
  }

  .filled-line {
    position: absolute;
    left: 0; /* Démarre à gauche */
    width: 0; /* Largeur initiale */
    height: 2px; /* Même épaisseur que .line */
    background-color: white; /* Couleur de remplissage */
    top: 50%;
    transition: width 0.3s ease; /* Transition douce pour le remplissage */
  }
  .point {
    width: 10px;
    height: 10px;
    background-color: white;
    border-radius: 50%;
    position: absolute;
    /* Ajustement pour le centrage vertical */
    top: 50%;
    transform: translateY(-50%);
    z-index: 20;
  }

  .point-circle {
    width: 10px;
    height: 10px;
    background-color: white;
    border-radius: 50%;
    transition: transform 0.3s ease; /* Animation pour l'agrandissement */
  }

  .point-circle-active {
    transform: scale(1.5); /* Double la taille */
  }
  .point-circle:hover {
    transform: scale(1.5); /* Double la taille */
    background-color: rgb(255, 162, 0);
    cursor: pointer;
    transition: transform 0.1s ease; /* Animation pour l'agrandissement */
  }

  .point-text {
    position: absolute;
    top: 12px;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    color: white;
    font-family: "TabletGothicWideRegular", sans-serif;
  }

  .logo-image {
    position: fixed; /* Fixed position */
    top: 30px; /* Position from the top */
    left: 30px; /* Position from the right */
    width: 150px; /* Adjust the width as necessary */
    height: auto; /* Maintain aspect ratio */
    z-index: 1000; /* Ensure the image stays on top of other elements */
  }

  /* Media queries pour ajuster les marges sur des écrans plus petits */
  @media (max-width: 768px) {
    .point {
      margin: 0 5vw; /* Espacement plus important entre les points pour les écrans plus petits */
    }
  }
</style>
