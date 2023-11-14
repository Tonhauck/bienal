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
      image: "/img/img_satellite/Buenos_Aires.png",
      y: -34.6037,
      x: -58.3816,
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
    },
    {
      KM: "Km 31.1",
      Ciudad: "Tigre",
      País: "Argentina",
      y: -34.4251,
      x: -58.5796,
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
    {
      KM: "Km 171",
      Ciudad: "San José",
      País: "Uruguay",
      y: -34.3371,
      x: -56.7135,
    },
    {
      KM: "Km 199",
      Ciudad: "Montevideo",
      País: "Uruguay",
      y: -34.9011,
      x: -56.1915,
    },
    {
      KM: "Km 235",
      Ciudad: "Junín",
      País: "Argentina",
      y: -34.585,
      x: -60.9589,
    },
    {
      KM: "Km 278",
      Ciudad: "Rosario",
      País: "Argentina",
      y: -32.9442,
      x: -60.6505,
    },
    {
      KM: "Km 312",
      Ciudad: "Los Toldos",
      País: "Argentina",
      y: -34.6103,
      x: -61.1734,
    },
    {
      KM: "Km 383",
      Ciudad: "Mar del Plata",
      País: "Argentina",
      y: -38.0055,
      x: -57.5426,
    },
    {
      KM: "Km 391",
      Ciudad: "Santa Fe",
      País: "Argentina",
      y: -31.6107,
      x: -60.6973,
    },
    {
      KM: "Km 524",
      Ciudad: "Treinta y tres",
      País: "Uruguay",
      y: -33.2315,
      x: -54.3744,
    },
    {
      KM: "Km 644",
      Ciudad: "Córdoba",
      País: "Argentina",
      y: -31.4201,
      x: -64.1888,
    },
    {
      KM: "Km 664",
      Ciudad: "Villa de Merlo",
      País: "San Luis",
      y: -32.3428,
      x: -65.0235,
    },
    {
      KM: "Km 696",
      Ciudad: "Santa Maria",
      País: "Brasil",
      y: -29.6842,
      x: -53.8069,
    },
    {
      KM: "Km 978",
      Ciudad: "San Fernando del Valle de Catamarca",
      País: "Argentina",
      y: -28.4696,
      x: -65.7852,
    },
    {
      KM: "Km 988.7",
      Ciudad: "Mendoza",
      País: "Argentina",
      y: -32.8895,
      x: -68.8458,
    },
    {
      KM: "Km 988.9",
      Ciudad: "Neuquén",
      País: "Argentina",
      y: -38.9516,
      x: -68.0591,
    },
    {
      KM: "Km 1005",
      Ciudad: "San Juan",
      País: "Argentina",
      y: -31.5375,
      x: -68.5364,
    },
    {
      KM: "Km 1037",
      Ciudad: "Asunción",
      País: "Paraguay",
      y: -25.2637,
      x: -57.5759,
    },
    {
      KM: "Km 1049",
      Ciudad: "Santiago del Estero",
      País: "Argentina",
      y: -27.7951,
      x: -64.2615,
    },
    {
      KM: "Km 1081",
      Ciudad: "San Miguel de Tucumán",
      País: "Argentina",
      y: -26.8083,
      x: -65.2176,
    },
    {
      KM: "Km 1137",
      Ciudad: "Providencia",
      País: "Chile",
      y: -33.4288,
      x: -70.611,
    },
    {
      KM: "Km 1138",
      Ciudad: "Santiago de Chile",
      País: "Chile",
      y: -33.4489,
      x: -70.6693,
    },
    {
      KM: "Km 1236",
      Ciudad: "Valparaíso",
      País: "Chile",
      y: -33.0458,
      x: -71.6197,
    },
    {
      KM: "Km 1247",
      Ciudad: "Monteros",
      País: "Argentina",
      y: -27.167,
      x: -65.4983,
    },
    {
      KM: "Km 1285",
      Ciudad: "Salta",
      País: "Argentina",
      y: -24.7821,
      x: -65.4232,
    },
    {
      KM: "Km 1344",
      Ciudad: "Curitiba",
      País: "Brasil",
      y: -25.4809,
      x: -49.3044,
    },
    {
      KM: "Km 1401",
      Ciudad: "Tilcara",
      País: "Argentina",
      y: -23.5776,
      x: -65.3509,
    },
    {
      KM: "Km 1548",
      Ciudad: "Chubut",
      País: "Argentina",
      y: -43.2934,
      x: -65.1115,
    },
    {
      KM: "Km 1820",
      Ciudad: "Potosí",
      País: "Bolivia",
      y: -19.5723,
      x: -65.755,
    },
    {
      KM: "Km 1834",
      Ciudad: "Valdivia",
      País: "Chile",
      y: -39.8196,
      x: -73.2452,
    },
    {
      KM: "Km 1928",
      Ciudad: "Santa Cruz de la Sierra",
      País: "Bolivia",
      y: -17.7863,
      x: -63.1812,
    },
    {
      KM: "Km 1962",
      Ciudad: "Río de Janeiro",
      País: "Brasil",
      y: -22.9068,
      x: -43.1729,
    },
    {
      KM: "Km 2004",
      Ciudad: "Porto Alegre",
      País: "Brasil",
      y: -30.0346,
      x: -51.2177,
    },
    {
      KM: "Km 2083",
      Ciudad: "Río Gallegos",
      País: "Argentina",
      y: -51.623,
      x: -69.2168,
    },
    {
      KM: "Km 2155",
      Ciudad: "Sorocaba",
      País: "Brasil",
      y: -23.5015,
      x: -47.4526,
    },
    {
      KM: "Km 2233",
      Ciudad: "San Pablo",
      País: "Brasil",
      y: -23.5505,
      x: -46.6333,
    },
    {
      KM: "Km 2234",
      Ciudad: "La Paz",
      País: "Bolivia",
      y: -16.4897,
      x: -68.1193,
    },
    {
      KM: "Km 2337",
      Ciudad: "Brasilia",
      País: "Brasil",
      y: -15.7942,
      x: -47.8822,
    },
    {
      KM: "Km 2748",
      Ciudad: "Punta Arenas",
      País: "Chile",
      y: -53.1638,
      x: -70.9171,
    },
    {
      KM: "Km 2899",
      Ciudad: "Río Grande",
      País: "Argentina",
      y: -53.7877,
      x: -67.7095,
    },
    {
      KM: "Km 2900",
      Ciudad: "Zona fronteriza entre Arica y Tacna",
      País: "Chile / Perú",
      y: -18.4746,
      x: -70.2979,
    },
    {
      KM: "Km 2989",
      Ciudad: "Ushuaia",
      País: "Argentina",
      y: -54.8019,
      x: -68.3029,
    },
    {
      KM: "Km 3055",
      Ciudad: "Puerto Almanza",
      País: "Argentina",
      y: -54.8526,
      x: -68.4202,
    },
    {
      KM: "Km 3127",
      Ciudad: "Lima",
      País: "Perú",
      y: -12.0464,
      x: -77.0428,
    },
    {
      KM: "Km 4120",
      Ciudad: "Cuenca",
      País: "Ecuador",
      y: -2.9001,
      x: -79.0059,
    },
    {
      KM: "Km 4236",
      Ciudad: "Guayaquil",
      País: "Ecuador",
      y: -2.17,
      x: -79.9224,
    },
    {
      KM: "Km 4658",
      Ciudad: "Bogotá",
      País: "Colombia",
      y: 4.71,
      x: -74.0721,
    },
    {
      KM: "Km 4951",
      Ciudad: "Villa del Rosario",
      País: "Colombia",
      y: 7.8339,
      x: -72.4745,
    },
    {
      KM: "Km 4951",
      Ciudad: "Cúcuta",
      País: "Colombia",
      y: 7.8977,
      x: -72.5077,
    },
    {
      KM: "Km 5356",
      Ciudad: "Barranquilla",
      País: "Colombia",
      y: 10.979,
      x: -74.8013,
    },
    {
      KM: "Km 6150",
      Ciudad: "Santiago De Los Caballeros",
      País: "República Dominicana",
      y: 19.4557,
      x: -70.707,
    },
    {
      KM: "Km 6440",
      Ciudad: "Ciudad de Guatemala",
      País: "Guatemala",
      y: 14.6349,
      x: -90.5069,
    },
    {
      KM: "Km 6975",
      Ciudad: "Dakar",
      País: "Senegal",
      y: 14.6928,
      x: -17.4467,
    },
    {
      KM: "Km 7286",
      Ciudad: "Puebla",
      País: "México",
      y: 19.0414,
      x: -98.2063,
    },
    {
      KM: "Km 7385",
      Ciudad: "Ciudad de México",
      País: "México",
      y: 19.4326,
      x: -99.1332,
    },
    {
      KM: "Km 7827",
      Ciudad: "Ouidah",
      País: "Benín",
      y: 6.369,
      x: 2.0856,
    },
    {
      KM: "Km 8138",
      Ciudad: "Houston",
      País: "Estados Unidos",
      y: 29.7604,
      x: -95.3698,
    },
    {
      KM: "Km 8305",
      Ciudad: "Las Palmas de Gran Canaria",
      País: "España",
      y: 28.1235,
      x: -15.4365,
    },
    {
      KM: "Km 8439",
      Ciudad: "Yaundé",
      País: "Camerún",
      y: 3.848,
      x: 11.5021,
    },
    {
      KM: "Km 9030",
      Ciudad: "Marrakech",
      País: "Marruecos",
      y: 31.6295,
      x: -7.9811,
    },
    {
      KM: "Km 9659",
      Ciudad: "Sevilla",
      País: "España",
      y: 37.3886,
      x: -5.9823,
    },
    {
      KM: "Km 9692",
      Ciudad: "Málaga",
      País: "España",
      y: 36.7213,
      x: -4.4214,
    },
    {
      KM: "Km 10026",
      Ciudad: "Madrid",
      País: "España",
      y: 40.4168,
      x: -3.7038,
    },
    {
      KM: "Km 10319",
      Ciudad: "Bilbao",
      País: "España",
      y: 43.263,
      x: -2.934,
    },
    {
      KM: "Km 10352.43",
      Ciudad: "Palma de Mallorca",
      País: "España",
      y: 39.5696,
      x: 2.6502,
    },
    {
      KM: "Km 10353",
      Ciudad: "Cala Figuera",
      País: "Mallorca",
      y: 39.3302,
      x: 3.1645,
    },
    {
      KM: "Km 10377",
      Ciudad: "San Sebastián",
      País: "España",
      y: 43.3183,
      x: -1.9812,
    },
    {
      KM: "Km 10440",
      Ciudad: "Barcelona",
      País: "España",
      y: 41.3851,
      x: 2.1734,
    },
    {
      KM: "Km 10625",
      Ciudad: "La Rochelle",
      País: "Francia",
      y: 46.1591,
      x: -1.152,
    },
    {
      KM: "Km 10681",
      Ciudad: "Angoulême",
      País: "Francia",
      y: 45.6484,
      x: 0.1562,
    },
    {
      KM: "Km 10779",
      Ciudad: "Rennes",
      País: "Francia",
      y: 48.1173,
      x: -1.6778,
    },
    {
      KM: "Km 10801",
      Ciudad: "Toulouse",
      País: "Francia",
      y: 43.6047,
      x: 1.4442,
    },
    {
      KM: "Km 11009",
      Ciudad: "Bastia",
      País: "Francia",
      y: 42.6973,
      x: 9.4509,
    },
    {
      KM: "Km 11020",
      Ciudad: "Vitry-sur-Seine",
      País: "Francia",
      y: 48.7872,
      x: 2.4033,
    },
    {
      KM: "Km 11022",
      Ciudad: "Marsella",
      País: "Francia",
      y: 43.2965,
      x: 5.3698,
    },
    {
      KM: "Km 11045",
      Ciudad: "París",
      País: "Francia",
      y: 48.8566,
      x: 2.3522,
    },
    {
      KM: "Km 11093",
      Ciudad: "Besanzón",
      País: "Francia",
      y: 47.2378,
      x: 6.0241,
    },
    {
      KM: "Km 11115",
      Ciudad: "Sélestat",
      País: "Francia",
      y: 48.2609,
      x: 7.4557,
    },
    {
      KM: "Km 11129",
      Ciudad: "Roma",
      País: "Italia",
      y: 41.9028,
      x: 12.4964,
    },
    {
      KM: "Km 11343",
      Ciudad: "Matera",
      País: "Italia",
      y: 40.6655,
      x: 16.6115,
    },
    {
      KM: "Km 11460",
      Ciudad: "Düsseldorf",
      País: "Alemania",
      y: 51.2277,
      x: 6.7735,
    },
    {
      KM: "Km 11594",
      Ciudad: "Geelong",
      País: "Australia",
      y: -38.1499,
      x: 144.3617,
    },
    {
      KM: "Km 11632",
      Ciudad: "Zagreb",
      País: "Croacia",
      y: 45.815,
      x: 15.9819,
    },
    {
      KM: "Km 11920",
      Ciudad: "Lens",
      País: "Suiza",
      y: 46.2957,
      x: 7.4398,
    },
    {
      KM: "Km 11921",
      Ciudad: "Crans-Montana",
      País: "Suiza",
      y: 46.3075,
      x: 7.4857,
    },
    {
      KM: "Km 11949",
      Ciudad: "Esmirna",
      País: "Turquía",
      y: 38.4192,
      x: 27.1287,
    },
    {
      KM: "Km 12009",
      Ciudad: "Jeddah",
      País: "Arabia Saudita",
      y: 21.5822,
      x: 39.1657,
    },
    {
      KM: "Km 12243",
      Ciudad: "Cisjordania",
      País: "Palestina",
      y: 32,
      x: 35.25,
    },
    {
      KM: "Km 12843",
      Ciudad: "Riyadh",
      País: "Arabia Saudita",
      y: 24.7136,
      x: 46.6753,
    },
    {
      KM: "Km 18370",
      Ciudad: "Tokio",
      País: "Japón",
      y: 35.6895,
      x: 139.6917,
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
  let multiplicatorPositionPointOnLine = 72;
  const kmValues = data.map((d) => parseFloat(d.KM.split(" ")[1] || 0));
  let initialStateKM = kmValues;

  function handleScroll() {
    const scrollPercentage =
      (window.scrollY / (document.body.scrollHeight - window.innerHeight)) *
      multiplicatorPositionPointOnLine;
    filledLineLength = scrollPercentage;
  }

  function calculatePointPositions() {
    // Créer une copie de data sans le premier élément
    let points = data.slice(1);

    let totalPoints = data.length;
    console.log(points);
    pointPositions = data.map(
      (_, index) => (index / totalPoints) * multiplicatorPositionPointOnLine
    );
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

  const setFlyTo = function (data, currentStep, onCompleted) {
    let flyToOptions;

    if (currentStep == 0) {
      flyToOptions = {
        bearing: 0,
        center: [data.x, data.y],
        zoom: 1.5,
        pitch: 0,
      };
      spinGlobe();
    } else {
      flyToOptions = {
        bearing: random(0, 90),
        center: [data.x, data.y],
        zoom: random(12, 14),
        pitch: random(30, 45),
      };
    }
    map.flyTo(flyToOptions);
    setTimeout(function () {
      map.once("moveend", onCompleted); // Écoute une seule fois l'événement 'moveend'
    }, 700);
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

  let isHandleClickAllowed = true;

  $: if (
    isHandleClickAllowed &&
    currentStep != -1 &&
    currentStep < data.length
  ) {
    console.log(isHandleClickAllowed);
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
    console.log("Point clicked, current step:", index);
    isHandleClickAllowed = false;
    currentStep = index;
    scrollToCurrentStep();
    setFlyTo(data[index], index, () => {
      data[index];
      index;
      isHandleClickAllowed = true;
    });
  }

  onDestroy(() => {
    window.removeEventListener("scroll", handleScroll);
  });
  onMount(() => {
    window.scrollTo(0, 0);

    // Réinitialiser currentStep à 0
    currentStep = -1;

    calculatePointPositions();
    window.addEventListener("scroll", handleScroll);

    if (currentStep >= 0) {
      scrollToCurrentStep();
    } else {
      currentStep == 0;
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
        map.easeTo({ center, duration: 700, easing: (n) => n });
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
      isHandleClickAllowed = true;
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
            <span
              class="point-text {i % 2 === 0
                ? 'point-text-above'
                : 'point-text-below'}"
            >
              {point.KM}
            </span>
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
    align-items: center;
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
    justify-content: space-between;
    align-items: center;
    padding: 10px;
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
    width: 400%;
    z-index: 13;
    position: fixed;
    background-color: #000;
    overflow-y: hidden;
    overflow-x: scroll; /* Permettre le défilement horizontal */
    scrollbar-width: none; /* Pour les navigateurs supportant cette propriété */
    -ms-overflow-style: none; /* Pour Internet Explorer 10+ */
  }
  .center-container::-webkit-scrollbar {
    display: none; /* Pour les navigateurs WebKit comme Chrome et Safari */
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
    width: 200%;
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

    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    color: white;
    font-family: "TabletGothicWideRegular", sans-serif;
  }

  .point-text-above {
    bottom: 100%; /* Positionner au-dessus du point-circle */
    margin-bottom: 5px; /* Espacement entre le texte et le point-circle */
  }

  .point-text-below {
    top: 100%; /* Positionner en dessous du point-circle */
    margin-top: 5px; /* Espacement entre le texte et le point-circle */
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

  .point-text {
    /* Taille de texte de base pour les grands écrans */
    font-size: 14px;
  }

  /* Taille de texte plus petite pour les écrans moyens */
  @media (max-width: 768px) {
    .point-text {
      font-size: 10px;
    }
  }

  /* Taille de texte encore plus petite pour les petits écrans */
  @media (max-width: 480px) {
    .point-text {
      font-size: 12px;
    }
  }
</style>
