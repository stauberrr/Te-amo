1. Archivo: index.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Corazón Mágico</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="space-background">
        <div class="floating-elements">
            </div>
        <div class="asteroid-container">
            </div>
    </div>

    <div class="heart" id="magicHeart"></div>
    <div class="love-text" id="loveText">Te amo</div>

    <script src="script.js"></script>
</body>
</html>

2. Archivo: style.css
body {
    margin: 0;
    overflow: hidden; /* Oculta cualquier elemento que se salga de la pantalla */
    height: 100vh; /* Altura completa de la ventana */
    background: radial-gradient(ellipse at bottom, #1b2735 0%, #090a0f 100%); /* Fondo espacial */
    position: relative; /* Para posicionar elementos absolutos dentro */
    cursor: default; /* Cursor por defecto */
}

.space-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 0; /* Asegura que esté detrás de todo */
}

/* Diseño del Corazón (usando pseudo-elementos para la forma) */
.heart {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100px;
    height: 90px;
    background-color: #e2264d; /* Color inicial rojo */
    transform-origin: 50% 50%; /* Punto central para transformaciones */
    cursor: pointer;
    z-index: 10; /* Asegura que esté por encima del fondo */
    transition: opacity 0.5s ease, transform 0.5s ease; /* Transición suave para reaparición */
}

.heart::before,
.heart::after {
    content: '';
    position: absolute;
    width: 100px;
    height: 60px;
    background-color: inherit; /* Hereda el color del .heart */
    border-radius: 50px 50px 0 0;
}

.heart::before {
    top: -50px;
    left: 0;
    transform: rotate(-45deg);
    transform-origin: 0 100%;
}

.heart::after {
    top: -50px;
    right: 0;
    transform: rotate(45deg);
    transform-origin: 100% 100%;
}

/* Clase para la animación de explosión */
.heart.exploding {
    animation: explode 0.5s ease-out forwards;
    opacity: 0; /* Ocultar durante la explosión */
}

/* Animación de explosión */
@keyframes explode {
    0% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
    100% { transform: translate(-50%, -50%) scale(2); opacity: 0; }
}

/* Texto "Te amo" */
.love-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 2em;
    font-family: 'Arial', sans-serif; /* O la fuente que prefieras */
    opacity: 0; /* Inicialmente invisible */
    transition: opacity 0.3s ease-in-out;
    pointer-events: none; /* Para que no interfiera con el click del corazón */
    z-index: 5; /* Entre el fondo y el corazón */
}

/* Elementos flotantes (planetas) */
.floating-elements {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none; /* Ignorar clicks */
}

.planet {
    position: absolute;
    width: 50px; /* Tamaño inicial, se puede variar */
    height: 50px; /* Tamaño inicial, se puede variar */
    background-color: rgba(100, 150, 250, 0.5); /* Color de ejemplo, puedes usar imágenes */
    border-radius: 50%; /* Forma circular */
    opacity: 0.7;
    animation: float 10s ease-in-out infinite alternate; /* Animación flotante */
}

/* Animación de flotar para planetas */
@keyframes float {
    0% { transform: translate(0, 0); }
    100% { transform: translate(20px, 10px); } /* Movimiento sutil */
}

/* Contenedor de asteroides */
.asteroid-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden; /* Esconde los asteroides hasta que entran en pantalla */
    pointer-events: none; /* Ignorar clicks */
}

/* Estilo básico del asteroide */
.asteroid {
    position: absolute;
    width: 10px; /* Tamaño del asteroide */
    height: 10px; /* Tamaño del asteroide */
    background-color: #ccc; /* Color grisáceo */
    border-radius: 2px; /* Forma cuadrada o irregular */
    opacity: 0.8;
    transform: translateX(100vw); /* Inicialmente fuera de pantalla a la derecha */
    will-change: transform; /* Optimización para animación */
}

/* La animación del asteroide se aplica en JavaScript */

3. Archivo: script.js
document.addEventListener('DOMContentLoaded', () => {
    const heart = document.getElementById('magicHeart');
    const loveText = document.getElementById('loveText');
    const asteroidContainer = document.querySelector('.asteroid-container');
    const floatingElementsContainer = document.querySelector('.floating-elements');

    // Colores alternativos para el corazón
    const alternativeColors = [
        '#3498db', // Azul
        '#2ecc71', // Verde
        '#f1c40f', // Amarillo
        '#9b59b6', // Morado
        '#e74c3c'  // Naranja/Rojo diferente
    ];
    let currentColorIndex = -1; // Para empezar con el color original

    // Función para cambiar el color del corazón
    function changeHeartColor() {
        currentColorIndex = (currentColorIndex + 1) % alternativeColors.length;
        heart.style.backgroundColor = alternativeColors[currentColorIndex];
        // Los pseudo-elementos heredan el color si su background-color es 'inherit'
    }

    // Evento al hacer click/tocar el corazón
    heart.addEventListener('click', () => {
        // Evita clicks múltiples rápidos
        if (heart.classList.contains('exploding')) {
            return;
        }

        // 1. Animación de explosión (CSS)
        heart.classList.add('exploding');
        heart.style.opacity = 0; // Asegura que se oculte al explotar

        // 2. Mostrar texto "Te amo"
        loveText.style.opacity = 1;

        // 3. Cambiar color y reaparecer después de la animación
        // El tiempo debe coincidir con la duración de la animación 'explode' en CSS (0.5s)
        setTimeout(() => {
            // Elimina la clase de explosión
            heart.classList.remove('exploding');

            // Cambia el color
            changeHeartColor();

            // Restablece el corazón (visible y tamaño normal)
            heart.style.opacity = 1; // Hacer visible de nuevo
            // La transición CSS maneja el escalado de vuelta si es necesario,
            // pero la animación explode establece forwards, así que reseteamos la transformación si usamos una más compleja
            // Para esta simple escala y opacidad, solo resetear la opacidad y la clase exploding es suficiente.

            // Ocultar el texto "Te amo" después de un breve momento
            setTimeout(() => {
                 loveText.style.opacity = 0;
            }, 800); // Tiempo visible del texto (ej: 0.8 segundos)

        }, 500); // Duración de la animación de explosión
    });

    // --- Funcionalidad del Fondo Espacial ---

    // Crear Planetas flotantes (ejemplo simple con divs)
    function createPlanets(numPlanets = 5) {
        for (let i = 0; i < numPlanets; i++) {
            const planet = document.createElement('div');
            planet.classList.add('planet');

            // Posición aleatoria (dentro de los límites, evitando el centro donde está el corazón)
            let randomX, randomY;
            do {
                randomX = Math.random() * window.innerWidth;
                randomY = Math.random() * window.innerHeight;
            } while (
                randomX > window.innerWidth * 0.4 && randomX < window.innerWidth * 0.6 &&
                randomY > window.innerHeight * 0.4 && randomY < window.innerHeight * 0.6
            ); // Evita el centro

            planet.style.left = `${randomX}px`;
            planet.style.top = `${randomY}px`;

            // Tamaño aleatorio
            const size = Math.random() * 30 + 20; // Entre 20px y 50px
            planet.style.width = `${size}px`;
            planet.style.height = `${size}px`;

            // Color aleatorio simple (puedes mejorar esto)
            const hue = Math.random() * 360;
            planet.style.backgroundColor = `hsla(${hue}, 70%, 50%, 0.6)`;

            // Retraso y duración de animación aleatorios para variación
            planet.style.animationDelay = `${Math.random() * 2}s`;
            planet.style.animationDuration = `${Math.random() * 8 + 8}s`; // Entre 8s y 16s

            floatingElementsContainer.appendChild(planet);
        }
    }

    // Crear Asteroide (animación manejada por CSS y JS)
    function createAsteroid() {
        const asteroid = document.createElement('div');
        asteroid.classList.add('asteroid');

        // Posición inicial a la derecha, altura aleatoria
        const startX = window.innerWidth;
        const startY = Math.random() * window.innerHeight;
        asteroid.style.left = `${startX}px`;
        asteroid.style.top = `${startY}px`;

        // Tamaño aleatorio
         const size = Math.random() * 8 + 5; // Entre 5px y 13px
         asteroid.style.width = `${size}px`;
         asteroid.style.height = `${size}px`;
         asteroid.style.borderRadius = `${Math.random() * 50}%`; // Para variar la forma un poco

        // Destino aleatorio a la izquierda (puede variar la altura ligeramente)
        const endX = -50; // Fuera de pantalla a la izquierda
        const endY = startY + (Math.random() - 0.5) * 100; // Pequeña variación vertical

        // Duración de la animación aleatoria para variar la velocidad
        const duration = Math.random() * 3 + 4; // Entre 4s y 7s

        // Aplicar animación CSS mediante keyframes generados dinámicamente o aplicar transformaciones
        // Usaremos la animación definida en CSS y ajustaremos la duración y posición inicial/final si es necesario
        // O, más simple, aplicamos la animación directamente vía style
         asteroid.style.animation = `moveAsteroid ${duration}s linear forwards`;

         // Definir keyframes directamente en el JS para controlar start/end mejor
         const styleSheet = document.styleSheets[0];
         const keyframesName = `asteroidMove-${Date.now()}-${Math.random()}`; // Nombre único
         const keyframesRule = `@keyframes ${keyframesName} {
             0% { transform: translate(${startX}px, ${startY}px) rotate(0deg); }
             100% { transform: translate(${endX}px, ${endY}px) rotate(360deg); } /* Rotación opcional */
         }`;

         styleSheet.insertRule(keyframesRule, styleSheet.cssRules.length);
         asteroid.style.animation = `${keyframesName} ${duration}s linear forwards`;


        // Eliminar el asteroide después de que termine la animación
        asteroid.addEventListener('animationend', () => {
            asteroid.remove();
        });

        asteroidContainer.appendChild(asteroid);
    }

    // Intervalo para crear nuevos asteroides cada 6 segundos
    setInterval(createAsteroid, 6000); // 6000 milisegundos = 6 segundos

    // Crear algunos planetas al cargar la página
    createPlanets(7); // Puedes ajustar la cantidad de planetas
});
