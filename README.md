# 🎮 UTNG Runner - Manual Completo del Proyecto

## 📚 Tabla de Contenidos
1. [Introducción al Proyecto](#introducción-al-proyecto)
2. [Conceptos Fundamentales](#conceptos-fundamentales)
3. [Arquitectura del Proyecto](#arquitectura-del-proyecto)
4. [Configuración Inicial](#configuración-inicial)
5. [Código Completo de Todos los Archivos](#código-completo-de-todos-los-archivos)
6. [Explicación Detallada de Cada Componente](#explicación-detallada-de-cada-componente)
7. [Cómo Funciona el Juego](#cómo-funciona-el-juego)
8. [Buenas Prácticas Aplicadas](#buenas-prácticas-aplicadas)

---

## 🎯 Introducción al Proyecto

### ¿Qué vamos a construir?

Vamos a crear un juego educativo llamado **"UTNG Runner"**, inspirado en el famoso dinosaurio de Google Chrome, pero con un mensaje social importante. En lugar de un dinosaurio, nuestro protagonista será un **alumno de la UTNG** que debe evitar obstáculos que representan hábitos nocivos para la salud:

- 🍔 **Comida chatarra** (mala alimentación)
- 🍺 **Alcohol**
- 💊 **Drogas**

### Analogía de la Vida Real

Imagina que estás caminando por tu día a día en la universidad. En tu camino encuentras diferentes tentaciones y decisiones que debes tomar:

- **Saltando sobre los obstáculos** = Tomar decisiones saludables
- **Chocar con obstáculos** = Consecuencias de malas decisiones
- **Contador de puntos** = Tu progreso y éxito académico

Este juego es como la vida real: mientras más decisiones saludables tomes, más lejos llegarás en tu carrera universitaria.

---

## 🧠 Conceptos Fundamentales

### ¿Qué es Jetpack Compose?

**Jetpack Compose** es la forma moderna de crear interfaces en Android. Piensa en ello como construir con bloques LEGO:

**Ejemplo de la Vida Diaria:**
- **Antes (XML)**: Era como construir una casa escribiendo instrucciones en un papel: "Pon una ventana aquí, una puerta allá"
- **Ahora (Compose)**: Es como tener bloques que puedes ver y modificar en tiempo real mientras los apilas

```kotlin
// Con Compose, describes lo que QUIERES ver, no CÓMO construirlo paso a paso
Text("Hola UTNG") // Esto crea un texto, ¡así de simple!
```

### ¿Qué es MVVM (Model-View-ViewModel)?

Es un patrón de arquitectura. Imagina una cocina de restaurante:

1. **Model (Modelo)**: Los ingredientes y recetas (los datos)
2. **View (Vista)**: El plato servido al cliente (lo que ve el usuario)
3. **ViewModel**: El chef que coordina todo (la lógica)

**Ventaja**: Si cambias el chef (ViewModel), los ingredientes (Model) y el plato (View) siguen funcionando. Todo está separado y organizado.

---

## 🏗️ Arquitectura del Proyecto

Nuestro proyecto está organizado en **capas**, como un edificio:

```
📦 com.utng.runner
 ┣ 📂 data (PLANTA BAJA - Datos)
 ┃ ┣ 📜 GameState.kt
 ┃ ┗ 📜 ObstacleType.kt
 ┣ 📂 domain (PRIMER PISO - Lógica del Negocio)
 ┃ ┗ 📜 GameEngine.kt
 ┣ 📂 presentation (SEGUNDO PISO - Presentación)
 ┃ ┣ 📂 components
 ┃ ┃ ┣ 📜 GameScreen.kt
 ┃ ┃ ┣ 📜 PlayerCharacter.kt
 ┃ ┃ ┣ 📜 Obstacle.kt
 ┃ ┃ ┗ 📜 GameOverDialog.kt
 ┃ ┗ 📜 GameViewModel.kt
 ┗ 📜 MainActivity.kt
```

**¿Por qué esta estructura?**

**Ejemplo de la Vida Real:** Es como organizar tu mochila escolar:
- **Datos (data)**: Cuadernos y apuntes ordenados por materia
- **Lógica (domain)**: Tu cerebro que decide qué estudiar y cuándo
- **Presentación (presentation)**: Cómo presentas tu tarea al profesor

Si todo está mezclado, es un caos. Si está organizado en capas, es fácil encontrar y modificar las cosas.

---

## ⚙️ Configuración Inicial

### 1. Archivo `build.gradle.kts` (Module: app)

Este archivo es como la lista de materiales para construir una casa.

```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
}

android {
    namespace = "com.utng.runner"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.utng.runner"
        minSdk = 24  // Compatible con Android 7.0 y superiores
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
        vectorDrawables {
            useSupportLibrary = true
        }
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    
    kotlinOptions {
        jvmTarget = "1.8"
    }
    
    buildFeatures {
        compose = true  // ¡Habilita Jetpack Compose!
    }
    
    composeOptions {
        kotlinCompilerExtensionVersion = "1.5.1"
    }
    
    packaging {
        resources {
            excludes += "/META-INF/{AL2.0,LGPL2.1}"
        }
    }
}

dependencies {
    // Jetpack Compose - El corazón de nuestra UI
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.7.0")
    implementation("androidx.activity:activity-compose:1.8.2")
    implementation(platform("androidx.compose:compose-bom:2024.02.00"))
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.ui:ui-graphics")
    implementation("androidx.compose.ui:ui-tooling-preview")
    implementation("androidx.compose.material3:material3")
    
    // ViewModel para Compose
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.7.0")
    
    // Testing
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
    androidTestImplementation(platform("androidx.compose:compose-bom:2024.02.00"))
    androidTestImplementation("androidx.compose.ui:ui-test-junit4")
    debugImplementation("androidx.compose.ui:ui-tooling")
    debugImplementation("androidx.compose.ui:ui-test-manifest")
}
```

**Explicación Detallada:**

- **namespace**: Es como el nombre completo de tu app (com.utng.runner)
- **compileSdk = 34**: La versión de Android que usaremos para compilar
- **minSdk = 24**: La app funcionará desde Android 7.0 en adelante
- **buildFeatures { compose = true }**: Le decimos a Android que usaremos Jetpack Compose

---

## 📁 Código Completo de Todos los Archivos

### 1️⃣ CAPA DE DATOS (Data Layer)

#### 📄 `data/ObstacleType.kt`

Este archivo define los tipos de obstáculos. Es como un catálogo de "malos hábitos".

```kotlin
package com.utng.runner.data

/**
 * ObstacleType representa los diferentes tipos de obstáculos (malos hábitos)
 * que el estudiante debe evitar en su camino universitario.
 * 
 * ANALOGÍA: Es como una lista de tentaciones que encuentras en tu día a día:
 * - La comida chatarra en el kiosko
 * - Las fiestas con alcohol
 * - Las drogas que algunos ofrecen
 * 
 * @property emoji El emoji que representa visualmente el obstáculo
 * @property name El nombre descriptivo del obstáculo
 * @property description Una breve descripción del riesgo que representa
 */
sealed class ObstacleType(
    val emoji: String,
    val name: String,
    val description: String
) {
    /**
     * Representa la comida chatarra y mala alimentación.
     * EJEMPLO: Comer pizza y hamburguesas todos los días en vez de comida balanceada
     */
    object JunkFood : ObstacleType(
        emoji = "🍔",
        name = "Comida Chatarra",
        description = "La mala alimentación afecta tu rendimiento académico"
    )

    /**
     * Representa el consumo de alcohol.
     * EJEMPLO: Las fiestas excesivas que te hacen perder clases y concentración
     */
    object Alcohol : ObstacleType(
        emoji = "🍺",
        name = "Alcohol",
        description = "El alcohol daña tu cerebro y tu futuro"
    )

    /**
     * Representa las drogas.
     * EJEMPLO: Cualquier sustancia que destruye tu salud y vida universitaria
     */
    object Drugs : ObstacleType(
        emoji = "💊",
        name = "Drogas",
        description = "Las drogas destruyen tu vida y sueños"
    )

    companion object {
        /**
         * Función que devuelve un obstáculo aleatorio.
         * 
         * ANALOGÍA: Es como girar una ruleta de tentaciones.
         * A veces te toca uno, a veces otro.
         * 
         * @return Un tipo de obstáculo seleccionado aleatoriamente
         */
        fun random(): ObstacleType {
            return when ((0..2).random()) {
                0 -> JunkFood
                1 -> Alcohol
                else -> Drugs
            }
        }
    }
}
```

**Explicación Detallada:**

**¿Qué es `sealed class`?**

Un `sealed class` es como una familia cerrada. Solo puede tener hijos específicos que tú defines.

**Ejemplo de la Vida Real:**
```
Familia "Obstáculos" (sealed class)
├── Hijo 1: Comida Chatarra
├── Hijo 2: Alcohol
└── Hijo 3: Drogas

No puede haber un hijo 4 sorpresa.
```

**Ventajas:**
1. El compilador sabe TODOS los tipos posibles
2. No puedes crear tipos nuevos por accidente
3. Es perfecto para representar un conjunto fijo de opciones

---

#### 📄 `data/GameState.kt`

Este archivo es el "estado" del juego. Imagina que es como una foto instantánea de tu juego en cualquier momento.

```kotlin
package com.utng.runner.data

/**
 * GameState representa el estado completo del juego en cualquier momento.
 * 
 * ANALOGÍA: Es como el tablero de un juego de mesa.
 * Puedes ver todos los datos importantes de un vistazo:
 * - ¿Dónde está el jugador?
 * - ¿Dónde están los obstáculos?
 * - ¿Cuántos puntos llevas?
 * - ¿El juego está corriendo o terminó?
 * 
 * INMUTABILIDAD: Usamos 'data class' porque cada estado es como
 * una foto. No modificamos la foto, creamos una nueva foto con los cambios.
 * 
 * @property playerY Posición vertical del jugador (altura del salto)
 * @property obstacles Lista de todos los obstáculos en pantalla
 * @property score Puntuación actual del jugador
 * @property isGameOver Si el juego ha terminado o no
 * @property isJumping Si el jugador está saltando actualmente
 * @property gameSpeed Velocidad del juego (aumenta con el tiempo)
 */
data class GameState(
    val playerY: Float = 0f,           // 0f = en el suelo, >0 = en el aire
    val obstacles: List<Obstacle> = emptyList(),
    val score: Int = 0,
    val isGameOver: Boolean = false,
    val isJumping: Boolean = false,
    val gameSpeed: Float = 5f          // Píxeles por frame
) {
    companion object {
        /**
         * Altura máxima que puede alcanzar el jugador al saltar.
         * 
         * ANALOGÍA: Es como la altura que puedes alcanzar al saltar
         * en la vida real. No puedes saltar hasta el techo, hay un límite.
         */
        const val MAX_JUMP_HEIGHT = 200f
        
        /**
         * Velocidad inicial de salto (pixeles por frame).
         * 
         * CONCEPTO: Una velocidad negativa significa "hacia arriba" en pantalla.
         * En Android, Y=0 está arriba, y Y aumenta hacia abajo.
         */
        const val JUMP_VELOCITY = -15f
        
        /**
         * Gravedad aplicada al jugador (pixeles por frame).
         * 
         * CONCEPTO: La gravedad es siempre positiva (tira hacia abajo).
         * Es la fuerza que te hace volver al suelo después de saltar.
         */
        const val GRAVITY = 0.8f
    }
}

/**
 * Obstacle representa un obstáculo individual en el juego.
 * 
 * ANALOGÍA: Es como una persona u objeto que te encuentras en tu camino.
 * Tiene una posición (dónde está) y un tipo (qué es).
 * 
 * @property x Posición horizontal del obstáculo (de derecha a izquierda)
 * @property type Tipo de obstáculo (JunkFood, Alcohol, o Drugs)
 */
data class Obstacle(
    val x: Float,                      // Posición en el eje X
    val type: ObstacleType            // Qué tipo de obstáculo es
) {
    companion object {
        /**
         * Ancho visual del obstáculo en píxeles.
         * Usado para detectar colisiones.
         */
        const val WIDTH = 80f
        
        /**
         * Alto visual del obstáculo en píxeles.
         * Usado para detectar colisiones.
         */
        const val HEIGHT = 80f
    }
}
```

**Explicación Detallada:**

**¿Qué es una `data class`?**

Una `data class` es una clase especial de Kotlin que automáticamente crea funciones útiles:

```kotlin
// Con data class, obtienes GRATIS:
val state1 = GameState(score = 10)
val state2 = state1.copy(score = 20)  // ✅ Función copy()
println(state1)                        // ✅ toString() bonito
val equals = state1 == state2          // ✅ Comparación por valor
```

**Ejemplo de la Vida Real:**

Imagina que tienes una ficha de información:
```
Nombre: Juan
Edad: 20
Calificación: 90
```

Con `data class`:
- Puedes copiar la ficha y cambiar solo la calificación
- Puedes comparar dos fichas fácilmente
- Puedes imprimir la ficha de forma legible

**Sistema de Coordenadas en Android:**

```
(0,0) ────────────────► X (derecha)
 │
 │  🧑 Jugador aquí (playerY = 0)
 │
 │  ☁️ Jugador saltando (playerY = -100)
 │
 ▼
 Y (abajo)
```

---

### 2️⃣ CAPA DE DOMINIO (Domain Layer)

#### 📄 `domain/GameEngine.kt`

Este es el "cerebro" del juego. Aquí está toda la lógica.

```kotlin
package com.utng.runner.domain

import com.utng.runner.data.GameState
import com.utng.runner.data.Obstacle
import com.utng.runner.data.ObstacleType

/**
 * GameEngine es el motor del juego, la lógica central.
 * 
 * ANALOGÍA: Es como el árbitro en un partido de fútbol.
 * El árbitro no juega, pero:
 * - Decide si hay falta (colisión)
 * - Cuenta los puntos
 * - Controla el tiempo
 * - Hace cumplir las reglas
 * 
 * RESPONSABILIDADES:
 * 1. Actualizar el estado del juego cada frame
 * 2. Detectar colisiones
 * 3. Generar obstáculos
 * 4. Calcular la física del salto
 * 5. Incrementar la puntuación
 * 
 * PRINCIPIO DE RESPONSABILIDAD ÚNICA:
 * Esta clase SOLO se encarga de la lógica del juego.
 * No sabe nada sobre la UI (botones, colores, etc.)
 */
object GameEngine {

    /**
     * Ancho de la pantalla del juego en píxeles lógicos.
     * Los obstáculos aparecen desde aquí.
     */
    private const val SCREEN_WIDTH = 1080f

    /**
     * Distancia mínima entre obstáculos.
     * ANALOGÍA: Es como el espacio mínimo entre dos personas en una fila.
     */
    private const val MIN_OBSTACLE_DISTANCE = 300f

    /**
     * Actualiza el estado del juego en cada frame (fotograma).
     * 
     * ANALOGÍA: Es como actualizar la posición de todas las piezas
     * en un juego de ajedrez después de cada turno.
     * 
     * ¿QUÉ PASA EN CADA FRAME?
     * 1. Si el juego terminó, no hacemos nada
     * 2. Movemos los obstáculos hacia la izquierda
     * 3. Eliminamos obstáculos que salieron de pantalla
     * 4. Generamos nuevos obstáculos si es necesario
     * 5. Actualizamos la física del salto
     * 6. Detectamos colisiones
     * 7. Incrementamos la puntuación
     * 8. Aumentamos la velocidad gradualmente
     * 
     * @param currentState El estado actual del juego
     * @return Un nuevo estado del juego actualizado
     */
    fun updateGameState(currentState: GameState): GameState {
        // Si el juego terminó, devolvemos el estado sin cambios
        // ANALOGÍA: Si el partido terminó, no seguimos jugando
        if (currentState.isGameOver) return currentState

        // Paso 1: Mover todos los obstáculos hacia la izquierda
        // CONCEPTO: Restamos gameSpeed de la posición X
        // Si X disminuye, el objeto se mueve a la izquierda
        val movedObstacles = currentState.obstacles.map { obstacle ->
            obstacle.copy(x = obstacle.x - currentState.gameSpeed)
        }

        // Paso 2: Filtrar obstáculos que salieron de pantalla (x < -100)
        // ANALOGÍA: Como quitar del juego las piezas que cayeron de la mesa
        val visibleObstacles = movedObstacles.filter { it.x > -100 }

        // Paso 3: Generar nuevos obstáculos si es necesario
        // LÓGICA: Solo generamos si no hay obstáculos o si el último
        // está lo suficientemente lejos
        val obstacles = if (shouldSpawnObstacle(visibleObstacles)) {
            visibleObstacles + createNewObstacle()
        } else {
            visibleObstacles
        }

        // Paso 4: Actualizar la física del jugador
        val (newPlayerY, newIsJumping) = updatePlayerPhysics(
            currentState.playerY,
            currentState.isJumping
        )

        // Paso 5: Detectar colisiones
        // CONCEPTO: Verificamos si algún obstáculo está tocando al jugador
        val collision = detectCollision(newPlayerY, obstacles)

        // Paso 6: Calcular nueva puntuación
        // LÓGICA: Sumamos 1 punto por cada frame que sobrevivimos
        val newScore = if (collision) currentState.score else currentState.score + 1

        // Paso 7: Aumentar velocidad gradualmente
        // CONCEPTO: Cada 500 puntos, aumentamos 0.5 la velocidad
        // Esto hace el juego más difícil con el tiempo
        val newSpeed = calculateGameSpeed(newScore)

        // Devolvemos el nuevo estado del juego
        // INMUTABILIDAD: No modificamos currentState, creamos uno nuevo
        return currentState.copy(
            playerY = newPlayerY,
            obstacles = obstacles,
            score = newScore,
            isGameOver = collision,
            isJumping = newIsJumping,
            gameSpeed = newSpeed
        )
    }

    /**
     * Inicia un salto si el jugador está en el suelo.
     * 
     * ANALOGÍA: Es como cuando flexionas las piernas para saltar.
     * Solo puedes saltar si estás en el suelo, no en el aire.
     * 
     * @param currentState Estado actual del juego
     * @return Nuevo estado con el salto iniciado (o sin cambios si ya está saltando)
     */
    fun jump(currentState: GameState): GameState {
        // Solo permitimos saltar si:
        // 1. No estamos ya saltando
        // 2. El juego no ha terminado
        return if (!currentState.isJumping && !currentState.isGameOver) {
            currentState.copy(isJumping = true)
        } else {
            currentState  // No hacemos nada si ya está saltando
        }
    }

    /**
     * Reinicia el juego al estado inicial.
     * 
     * ANALOGÍA: Como reiniciar un juego de mesa,
     * volvemos todas las piezas a su posición inicial.
     * 
     * @return Un nuevo GameState con valores iniciales
     */
    fun resetGame(): GameState {
        return GameState()  // Estado inicial por defecto
    }

    /**
     * Actualiza la física del jugador (gravedad y salto).
     * 
     * FÍSICA DEL SALTO:
     * 1. Al saltar, el jugador tiene velocidad hacia arriba (negativa)
     * 2. La gravedad reduce esta velocidad gradualmente
     * 3. Eventualmente, la velocidad se vuelve positiva (cae)
     * 4. El jugador regresa al suelo (Y = 0)
     * 
     * ANALOGÍA: Es como lanzar una pelota al aire:
     * - Sube rápido al principio
     * - Pierde velocidad
     * - Se detiene en el punto más alto
     * - Cae de vuelta
     * 
     * @param currentY Posición Y actual del jugador
     * @param isJumping Si el jugador está saltando
     * @return Par de (nueva posición Y, si sigue saltando)
     */
    private fun updatePlayerPhysics(
        currentY: Float,
        isJumping: Boolean
    ): Pair<Float, Boolean> {
        // Si no está saltando, mantiene la posición en el suelo
        if (!isJumping) return Pair(0f, false)

        // Calculamos la nueva velocidad aplicando gravedad
        // velocity empieza en JUMP_VELOCITY (negativo, hacia arriba)
        // gravity es positivo, reduce la velocidad hacia arriba
        var velocity = GameState.JUMP_VELOCITY
        var newY = currentY + velocity

        // Aplicamos gravedad en cada iteración del salto
        // CONCEPTO: La gravedad es acumulativa
        velocity += GameState.GRAVITY

        // Si alcanzamos la altura máxima, empezamos a caer
        if (newY < -GameState.MAX_JUMP_HEIGHT) {
            newY = -GameState.MAX_JUMP_HEIGHT
            velocity = 0f  // Detenemos la velocidad hacia arriba
        }

        // Simulamos la caída aplicando gravedad
        // CONCEPTO: Mientras estemos en el aire (newY < 0), seguimos aplicando física
        while (newY < 0) {
            velocity += GameState.GRAVITY
            newY += velocity

            // Si tocamos el suelo, terminamos el salto
            if (newY >= 0) {
                return Pair(0f, false)  // De vuelta al suelo
            }
        }

        return Pair(newY, true)  // Seguimos en el aire
    }

    /**
     * Detecta si hay colisión entre el jugador y algún obstáculo.
     * 
     * CONCEPTO DE COLISIÓN:
     * Dos rectángulos chocan si sus áreas se superponen.
     * 
     * ANALOGÍA: Es como saber si dos cajas se están tocando.
     * Si las esquinas de una caja están dentro de la otra caja, chocan.
     * 
     * HITBOX DEL JUGADOR:
     * - X: 100 a 200 (ancho de 100px)
     * - Y: playerY a playerY + 100 (alto de 100px)
     * 
     * @param playerY Posición Y del jugador
     * @param obstacles Lista de obstáculos a verificar
     * @return true si hay colisión, false si está seguro
     */
    private fun detectCollision(
        playerY: Float,
        obstacles: List<Obstacle>
    ): Boolean {
        // Definimos la hitbox del jugador
        val playerLeft = 100f
        val playerRight = 200f
        val playerTop = playerY
        val playerBottom = playerY + 100f

        // Verificamos colisión con cada obstáculo
        return obstacles.any { obstacle ->
            // Definimos la hitbox del obstáculo
            val obstacleLeft = obstacle.x
            val obstacleRight = obstacle.x + Obstacle.WIDTH
            val obstacleTop = 0f  // Los obstáculos están en el suelo
            val obstacleBottom = Obstacle.HEIGHT

            // Lógica de colisión AABB (Axis-Aligned Bounding Box)
            // HAY COLISIÓN SI:
            // 1. El lado derecho del jugador está a la derecha del lado izquierdo del obstáculo
            // 2. El lado izquierdo del jugador está a la izquierda del lado derecho del obstáculo
            // 3. El fondo del jugador está abajo del tope del obstáculo
            // 4. El tope del jugador está arriba del fondo del obstáculo
            playerRight > obstacleLeft &&
            playerLeft < obstacleRight &&
            playerBottom > obstacleTop &&
            playerTop < obstacleBottom
        }
    }

    /**
     * Determina si debemos generar un nuevo obstáculo.
     * 
     * LÓGICA:
     * - Si no hay obstáculos, generamos uno
     * - Si el último obstáculo está suficientemente lejos, generamos otro
     * 
     * ANALOGÍA: Es como poner más conos en una pista de obstáculos.
     * Solo pones uno nuevo cuando hay suficiente espacio.
     * 
     * @param obstacles Lista actual de obstáculos
     * @return true si debemos crear un nuevo obstáculo
     */
    private fun shouldSpawnObstacle(obstacles: List<Obstacle>): Boolean {
        if (obstacles.isEmpty()) return true

        // Obtenemos el obstáculo más a la derecha (el último generado)
        val lastObstacle = obstacles.maxByOrNull { it.x } ?: return true

        // Verificamos si está lo suficientemente lejos
        return (SCREEN_WIDTH - lastObstacle.x) > MIN_OBSTACLE_DISTANCE
    }

    /**
     * Crea un nuevo obstáculo en el borde derecho de la pantalla.
     * 
     * @return Un nuevo obstáculo con tipo aleatorio
     */
    private fun createNewObstacle(): Obstacle {
        return Obstacle(
            x = SCREEN_WIDTH,              // Aparece en el borde derecho
            type = ObstacleType.random()   // Tipo aleatorio
        )
    }

    /**
     * Calcula la velocidad del juego basada en la puntuación.
     * 
     * CONCEPTO: Dificultad progresiva
     * Mientras más tiempo sobrevives, más rápido va el juego.
     * 
     * FÓRMULA:
     * velocidad = velocidad_base + (puntos / 500) * 0.5
     * 
     * EJEMPLO:
     * - 0 puntos: velocidad = 5.0
     * - 500 puntos: velocidad = 5.5
     * - 1000 puntos: velocidad = 6.0
     * - 2000 puntos: velocidad = 7.0
     * 
     * @param score Puntuación actual
     * @return Nueva velocidad del juego
     */
    private fun calculateGameSpeed(score: Int): Float {
        val baseSpeed = 5f
        val speedIncrease = (score / 500) * 0.5f
        return baseSpeed + speedIncrease
    }
}
```

**Explicación Detallada del Motor de Física:**

**¿Cómo funciona la física del salto?**

Imagina que lanzas una pelota hacia arriba:

1. **Inicio**: La pelota tiene mucha velocidad hacia arriba (JUMP_VELOCITY = -15)
2. **Subida**: La gravedad reduce la velocidad poco a poco
3. **Punto máximo**: La velocidad llega a 0 por un instante
4. **Bajada**: La gravedad hace que la pelota caiga más rápido
5. **Suelo**: La pelota regresa a donde empezó

En código:
```kotlin
Frame 1: velocity = -15, Y = -15
Frame 2: velocity = -14.2, Y = -29.2
Frame 3: velocity = -13.4, Y = -42.6
...
Frame X: velocity = 0, Y = -200 (punto máximo)
...
Frame Y: velocity = +15, Y = 0 (de vuelta al suelo)
```

---

### 3️⃣ CAPA DE PRESENTACIÓN (Presentation Layer)

#### 📄 `presentation/GameViewModel.kt`

El ViewModel es el coordinador entre la lógica y la UI.

```kotlin
package com.utng.runner.presentation

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.utng.runner.data.GameState
import com.utng.runner.domain.GameEngine
import kotlinx.coroutines.delay
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.asStateFlow
import kotlinx.coroutines.launch

/**
 * GameViewModel es el "controlador" de nuestro juego.
 * 
 * ANALOGÍA: Es como el director técnico de un equipo de fútbol.
 * - Los jugadores (Views) hacen lo que el director dice
 * - El director (ViewModel) toma decisiones basadas en las reglas (GameEngine)
 * - El director no juega, solo coordina
 * 
 * RESPONSABILIDADES:
 * 1. Mantener el estado del juego
 * 2. Actualizar el juego cada frame
 * 3. Responder a las acciones del usuario (toques)
 * 4. Coordinar con el GameEngine
 * 
 * MVVM (Model-View-ViewModel):
 * - Model: GameState, ObstacleType (datos)
 * - View: GameScreen, componentes UI (lo que ve el usuario)
 * - ViewModel: Esta clase (coordinador)
 * 
 * VENTAJAS:
 * - La UI puede cambiar sin afectar la lógica
 * - Podemos testear la lógica sin la UI
 * - El estado sobrevive a rotaciones de pantalla
 */
class GameViewModel : ViewModel() {

    /**
     * _gameState es el estado PRIVADO (mutable)
     * Solo el ViewModel puede modificarlo
     * 
     * CONCEPTO: Principio de encapsulación
     * Nadie de afuera puede cambiar el estado directamente
     */
    private val _gameState = MutableStateFlow(GameState())

    /**
     * gameState es el estado PÚBLICO (inmutable)
     * La UI puede observarlo pero no modificarlo
     * 
     * ANALOGÍA: Es como ver un partido por TV.
     * Puedes ver lo que pasa, pero no puedes cambiar el marcador.
     */
    val gameState: StateFlow<GameState> = _gameState.asStateFlow()

    /**
     * Frame rate del juego (60 FPS = 16.67ms por frame)
     * 
     * CONCEPTO: FPS (Frames Per Second)
     * - 60 FPS = actualizamos 60 veces por segundo
     * - Cada actualización tarda ~16ms
     * - El ojo humano no nota diferencia arriba de 60 FPS
     */
    private val frameDelayMillis = 16L

    /**
     * Indica si el bucle del juego está corriendo
     */
    private var isGameLoopRunning = false

    /**
     * Inicia el juego y el bucle de actualización.
     * 
     * CONCEPTO: Game Loop (Bucle del Juego)
     * Es el corazón de cualquier videojuego.
     * 
     * PSEUDOCÓDIGO:
     * ```
     * mientras juego_activo:
     *     procesar_input()     // Leer toques/teclas
     *     actualizar_lógica()  // Física, colisiones, IA
     *     renderizar()         // Dibujar en pantalla
     *     esperar(16ms)        // Mantener 60 FPS
     * ```
     * 
     * ANALOGÍA: Es como el latido del corazón del juego.
     * Cada "latido" actualiza todo: enemigos, jugador, puntos.
     */
    fun startGame() {
        // Reiniciamos el estado
        _gameState.value = GameEngine.resetGame()
        
        // Si ya hay un bucle corriendo, no iniciamos otro
        if (isGameLoopRunning) return
        
        isGameLoopRunning = true

        // viewModelScope: Coroutine que se cancela automáticamente
        // cuando el ViewModel es destruido
        viewModelScope.launch {
            // Bucle infinito hasta que el juego termine
            while (isGameLoopRunning && !_gameState.value.isGameOver) {
                // Actualizamos el estado usando el GameEngine
                _gameState.value = GameEngine.updateGameState(_gameState.value)
                
                // Esperamos 16ms para mantener 60 FPS
                // CONCEPTO: Frame pacing - mantener velocidad constante
                delay(frameDelayMillis)
            }
            // Cuando salimos del bucle, marcamos que ya no está corriendo
            isGameLoopRunning = false
        }
    }

    /**
     * Maneja el salto del jugador.
     * 
     * FLUJO:
     * 1. Usuario toca la pantalla
     * 2. La UI llama a este método
     * 3. Le pedimos al GameEngine que haga saltar al jugador
     * 4. Actualizamos el estado
     * 5. La UI reacciona automáticamente al cambio
     * 
     * CONCEPTO: Flujo unidireccional de datos
     * User Action → ViewModel → GameEngine → New State → UI Update
     */
    fun onJump() {
        _gameState.value = GameEngine.jump(_gameState.value)
    }

    /**
     * Reinicia el juego cuando el usuario presiona "Reintentar".
     * 
     * Similar a startGame() pero puede llamarse después de un Game Over
     */
    fun restartGame() {
        startGame()
    }

    /**
     * Limpia recursos cuando el ViewModel es destruido.
     * 
     * CONCEPTO: Lifecycle management
     * Es importante detener el bucle para no desperdiciar recursos.
     * 
     * ANALOGÍA: Es como apagar las luces cuando sales de una habitación.
     */
    override fun onCleared() {
        super.onCleared()
        isGameLoopRunning = false
    }
}
```

**Explicación Detallada:**

**¿Qué es StateFlow?**

`StateFlow` es como un canal de TV:
- El ViewModel es la estación que transmite (emite estados)
- La UI es el televisor que recibe y muestra (observa estados)
- Cuando cambia el programa (estado), el TV se actualiza automáticamente

```kotlin
// En el ViewModel:
_gameState.value = nuevoEstado  // ✅ Emitimos

// En la UI (Composable):
val state by viewModel.gameState.collectAsState()  // ✅ Recibimos
```

**¿Por qué usamos Coroutines?**

Las coroutines son como tener múltiples trabajadores en una cocina:

```kotlin
viewModelScope.launch {  // Trabajador 1: Game Loop
    while(true) {
        actualizar()
        delay(16)
    }
}

// Mientras tanto, el UI thread sigue libre para responder a toques
```

Sin coroutines, el juego "congelaría" la pantalla mientras actualiza.

---

#### 📄 `presentation/components/GameScreen.kt`

La pantalla principal del juego.

```kotlin
package com.utng.runner.presentation.components

import androidx.compose.foundation.background
import androidx.compose.foundation.gestures.detectTapGestures
import androidx.compose.foundation.layout.*
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.LaunchedEffect
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.getValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.input.pointer.pointerInput
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import com.utng.runner.presentation.GameViewModel

/**
 * GameScreen es la pantalla principal donde se juega.
 * 
 * COMPOSABLE: Una función que describe UI
 * - Recibe datos (parámetros)
 * - Devuelve UI (no explícitamente, sino por composición)
 * - Se recompone (redibuja) cuando los datos cambian
 * 
 * ANALOGÍA: Es como una receta de cocina.
 * Cada vez que la ejecutas con los mismos ingredientes (parámetros),
 * obtienes el mismo plato (UI).
 * 
 * @param viewModel El ViewModel que controla la lógica
 */
@Composable
fun GameScreen(viewModel: GameViewModel) {
    
    /**
     * Observamos el estado del juego.
     * 
     * CONCEPTO: Reactive Programming
     * Cuando gameState cambia, este Composable se "recompone" (redibuja)
     * 
     * collectAsState() convierte el Flow en un State observable por Compose
     */
    val gameState by viewModel.gameState.collectAsState()

    /**
     * LaunchedEffect inicia el juego solo una vez.
     * 
     * CONCEPTO: Side Effect
     * Es una acción que ocurre "al lado" de la UI, no es UI en sí.
     * 
     * La key "game_start" asegura que solo se ejecute una vez
     * (no cada vez que se recompone)
     */
    LaunchedEffect(key1 = "game_start") {
        viewModel.startGame()
    }

    /**
     * Box es un contenedor que apila elementos uno sobre otro.
     * 
     * ANALOGÍA: Como apilar papeles sobre un escritorio.
     * El último elemento dibujado está arriba de todos.
     * 
     * ESTRUCTURA:
     * - Fondo (cielo azul)
     * - Suelo (línea verde)
     * - Jugador
     * - Obstáculos
     * - HUD (puntuación)
     * - Dialog Game Over (si aplica)
     */
    Box(
        modifier = Modifier
            .fillMaxSize()  // Ocupa toda la pantalla
            .background(Color(0xFF87CEEB))  // Color celeste (cielo)
            .pointerInput(Unit) {
                // Detecta toques en toda la pantalla
                detectTapGestures {
                    viewModel.onJump()  // Cuando toca, salta
                }
            }
    ) {
        
        // SUELO: Línea verde en la parte inferior
        /**
         * Box con color verde que representa el suelo.
         * 
         * CONCEPTO: Layout positioning
         * - fillMaxWidth() = ocupa todo el ancho
         * - height(8.dp) = 8 píxeles de alto
         * - align(Alignment.BottomCenter) = pegado al fondo
         */
        Box(
            modifier = Modifier
                .fillMaxWidth()
                .height(8.dp)
                .align(Alignment.BottomCenter)
                .background(Color(0xFF228B22))  // Verde bosque
        )

        // JUGADOR: Nuestro personaje (alumno UTNG)
        /**
         * PlayerCharacter se posiciona dinámicamente según gameState.playerY
         * 
         * CONCEPTO: Data-driven UI
         * La posición del jugador viene del estado, no de animaciones manuales
         */
        PlayerCharacter(
            modifier = Modifier.align(Alignment.BottomStart),
            yOffset = gameState.playerY
        )

        // OBSTÁCULOS: Dibujamos cada obstáculo de la lista
        /**
         * Iteramos sobre todos los obstáculos y los dibujamos.
         * 
         * CONCEPTO: List rendering
         * Cada obstáculo es independiente pero comparte la misma lógica de dibujo
         */
        gameState.obstacles.forEach { obstacle ->
            ObstacleComponent(
                obstacle = obstacle,
                modifier = Modifier.align(Alignment.BottomStart)
            )
        }

        // HUD: Heads-Up Display (información en pantalla)
        /**
         * Mostramos la puntuación en la esquina superior derecha.
         * 
         * CONCEPTO: HUD (Heads-Up Display)
         * Información que siempre está visible sobre el juego
         * 
         * ANALOGÍA: Como el velocímetro en un coche.
         * Siempre visible pero no interfiere con la carretera.
         */
        Text(
            text = "Puntos: ${gameState.score}",
            fontSize = 24.sp,
            fontWeight = FontWeight.Bold,
            color = Color.White,
            modifier = Modifier
                .align(Alignment.TopEnd)  // Esquina superior derecha
                .padding(16.dp)
        )

        // INSTRUCCIONES: Texto de ayuda
        /**
         * Mostramos instrucciones en la parte superior.
         */
        Column(
            modifier = Modifier
                .align(Alignment.TopStart)
                .padding(16.dp)
        ) {
            Text(
                text = "UTNG Runner",
                fontSize = 32.sp,
                fontWeight = FontWeight.Bold,
                color = Color(0xFF1976D2)  // Azul UTNG
            )
            Spacer(modifier = Modifier.height(8.dp))
            Text(
                text = "Toca para saltar y evitar malos hábitos",
                fontSize = 14.sp,
                color = Color.DarkGray
            )
        }

        // GAME OVER DIALOG
        /**
         * Si el juego terminó, mostramos el diálogo de Game Over.
         * 
         * CONCEPTO: Conditional rendering
         * Solo dibujamos el diálogo si isGameOver es true
         */
        if (gameState.isGameOver) {
            GameOverDialog(
                score = gameState.score,
                onRestart = { viewModel.restartGame() }
            )
        }
    }
}
```

**Explicación Detallada:**

**¿Qué es Recomposition?**

Compose funciona así:

1. **Primera vez**: Ejecuta `GameScreen` y dibuja todo
2. **Cambio de estado**: `gameState.score` cambia de 100 a 101
3. **Recomposition**: Compose solo redibuja el `Text` de puntuación, no todo
4. **Resultado**: Actualización eficiente y rápida

**Analogía:**
Es como actualizar una pizarra. Si cambias un número, no borras todo y reescribes, solo borras ese número y escribes el nuevo.

**Sistema de Coordenadas en Compose:**

```kotlin
(0,0) ────────────────► X
 │  TopStart     TopEnd
 │
 │  CenterStart  Center  CenterEnd
 │
 │  BottomStart  BottomCenter  BottomEnd
 ▼
 Y
```

---

#### 📄 `presentation/components/PlayerCharacter.kt`

El personaje jugador.

```kotlin
package com.utng.runner.presentation.components

import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.offset
import androidx.compose.foundation.layout.size
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

/**
 * PlayerCharacter dibuja el alumno UTNG (nuestro personaje).
 * 
 * DISEÑO:
 * - Emoji: 🧑‍🎓 (estudiante)
 * - Tamaño: 100x100 dp
 * - Posición base: 100dp desde la izquierda, en el suelo
 * - Movimiento: Solo en Y (arriba/abajo al saltar)
 * 
 * CONCEPTO: Stateless Composable
 * Este componente NO tiene estado propio.
 * Solo recibe datos (yOffset) y los muestra.
 * 
 * VENTAJA: Fácil de testear y reutilizar.
 * 
 * @param modifier Modificador de Compose para customización
 * @param yOffset Desplazamiento vertical (negativo = arriba)
 */
@Composable
fun PlayerCharacter(
    modifier: Modifier = Modifier,
    yOffset: Float
) {
    /**
     * Box posiciona el emoji del jugador.
     * 
     * EXPLICACIÓN DE OFFSET:
     * - offset(x, y) mueve el componente desde su posición original
     * - 100.dp en X = posición fija desde la izquierda
     * - yOffset.dp en Y = se mueve según el salto
     * 
     * EJEMPLO:
     * - yOffset = 0: En el suelo
     * - yOffset = -100: 100dp arriba (saltando)
     * - yOffset = -200: En el punto máximo del salto
     */
    Box(
        modifier = modifier
            .offset(x = 100.dp, y = yOffset.dp)  // Posicionamiento dinámico
            .size(100.dp),  // Tamaño del jugador
        contentAlignment = Alignment.Center  // Centrar el emoji dentro del Box
    ) {
        /**
         * El emoji representa al estudiante de UTNG.
         * 
         * PERSONALIZACIÓN: Puedes cambiar el emoji por:
         * - 👨‍🎓 (estudiante hombre)
         * - 👩‍🎓 (estudiante mujer)
         * - 🧑‍💼 (profesional)
         * - O incluso usar una imagen personalizada
         */
        Text(
            text = "🧑‍🎓",
            fontSize = 60.sp  // Tamaño grande para que se vea bien
        )
    }
}
```

**Explicación Detallada:**

**¿Por qué usar offset en lugar de padding?**

```kotlin
// ❌ INCORRECTO: padding no mueve, solo añade espacio
modifier.padding(top = yOffset.dp)  

// ✅ CORRECTO: offset mueve el elemento
modifier.offset(y = yOffset.dp)  
```

**Ejemplo Visual:**

```
Sin saltar (yOffset = 0):
┌─────────┐
│   🧑‍🎓   │  ← En el suelo
└─────────┘
━━━━━━━━━━━  (suelo)

Saltando (yOffset = -100):
┌─────────┐
│   🧑‍🎓   │  ← 100dp arriba
└─────────┘

━━━━━━━━━━━  (suelo)
```

---

#### 📄 `presentation/components/Obstacle.kt`

Los obstáculos (malos hábitos).

```kotlin
package com.utng.runner.presentation.components

import androidx.compose.foundation.layout.*
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import com.utng.runner.data.Obstacle

/**
 * ObstacleComponent dibuja un obstáculo individual.
 * 
 * CONCEPTO: Component reusability
 * Este componente se reutiliza para TODOS los obstáculos.
 * Solo cambia su posición y emoji según el tipo.
 * 
 * ANALOGÍA: Es como una plantilla para estampas.
 * La plantilla es la misma, pero cada estampa puede tener diferente color/diseño.
 * 
 * @param obstacle El obstáculo a dibujar (contiene posición y tipo)
 * @param modifier Modificador de Compose
 */
@Composable
fun ObstacleComponent(
    obstacle: Obstacle,
    modifier: Modifier = Modifier
) {
    /**
     * Column apila elementos verticalmente.
     * Aquí apilamos el emoji y el texto descriptivo.
     * 
     * ESTRUCTURA:
     * ┌─────────┐
     * │  emoji  │  ← 🍔 o 🍺 o 💊
     * │  texto  │  ← "Comida Chatarra"
     * └─────────┘
     */
    Column(
        modifier = modifier
            // offset mueve el obstáculo horizontalmente
            // El eje X crece hacia la derecha
            .offset(x = obstacle.x.dp, y = 0.dp)
            .size(
                width = Obstacle.WIDTH.dp,
                height = Obstacle.HEIGHT.dp
            ),
        horizontalAlignment = Alignment.CenterHorizontally,  // Centrar contenido
        verticalArrangement = Arrangement.Center
    ) {
        /**
         * Emoji del obstáculo.
         * Cada tipo de obstáculo tiene su emoji único.
         */
        Text(
            text = obstacle.type.emoji,
            fontSize = 50.sp
        )
        
        /**
         * Nombre del obstáculo en texto pequeño.
         * Ayuda a identificar qué tipo de mal hábito es.
         */
        Text(
            text = obstacle.type.name,
            fontSize = 10.sp
        )
    }
}
```

**Explicación Detallada:**

**Flujo de Datos:**

```
GameEngine genera:
Obstacle(x=1080, type=JunkFood)
        ↓
GameViewModel actualiza:
obstacles = [Obstacle(x=1075), ...]
        ↓
GameScreen itera:
forEach { ObstacleComponent(it) }
        ↓
ObstacleComponent dibuja:
🍔 en posición X=1075dp
```

**Movimiento del Obstáculo:**

```
Frame 1: x = 1080 (fuera de pantalla derecha)
Frame 2: x = 1075 (entrando)
Frame 3: x = 1070
...
Frame N: x = -100 (salió por la izquierda)
```

---

#### 📄 `presentation/components/GameOverDialog.kt`

El diálogo cuando pierdes.

```kotlin
package com.utng.runner.presentation.components

import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import androidx.compose.ui.window.Dialog

/**
 * GameOverDialog muestra el diálogo cuando el jugador pierde.
 * 
 * CONCEPTO: Modal Dialog
 * Es una ventana que aparece sobre el juego y bloquea la interacción
 * hasta que el usuario tome una decisión.
 * 
 * ANALOGÍA: Como una alerta en tu teléfono.
 * No puedes hacer nada más hasta que la cierres o respondas.
 * 
 * CONTENIDO:
 * - Título "Game Over"
 * - Mensaje motivacional
 * - Puntuación final
 * - Botón para reiniciar
 * 
 * @param score Puntuación final del jugador
 * @param onRestart Callback que se ejecuta al presionar "Reintentar"
 */
@Composable
fun GameOverDialog(
    score: Int,
    onRestart: () -> Unit  // Función lambda sin parámetros que no devuelve nada
) {
    /**
     * Dialog es un componente de Material Design.
     * 
     * PROPIEDADES:
     * - onDismissRequest: Qué hacer al tocar fuera del diálogo
     *   (aquí lo dejamos vacío para que NO se pueda cerrar sin reiniciar)
     */
    Dialog(onDismissRequest = { /* No permitimos cerrar sin reiniciar */ }) {
        /**
         * Card personalizado para el contenido del diálogo.
         * 
         * DISEÑO:
         * - Fondo blanco
         * - Bordes redondeados
         * - Padding generoso
         */
        Box(
            modifier = Modifier
                .fillMaxWidth(0.9f)  // 90% del ancho de la pantalla
                .background(
                    color = Color.White,
                    shape = RoundedCornerShape(16.dp)  // Esquinas redondeadas
                )
                .padding(24.dp)  // Espacio interno
        ) {
            /**
             * Column organiza los elementos verticalmente.
             */
            Column(
                horizontalAlignment = Alignment.CenterHorizontally,
                verticalArrangement = Arrangement.spacedBy(16.dp)  // Espacio entre elementos
            ) {
                
                // TÍTULO "GAME OVER"
                Text(
                    text = "¡Game Over!",
                    fontSize = 32.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color(0xFFD32F2F)  // Rojo de advertencia
                )

                // EMOJI TRISTE
                Text(
                    text = "😢",
                    fontSize = 48.sp
                )

                // MENSAJE MOTIVACIONAL
                /**
                 * Mensaje que relaciona el juego con la vida real.
                 * 
                 * OBJETIVO EDUCATIVO:
                 * Reforzar el mensaje de tomar decisiones saludables.
                 */
                Text(
                    text = "Los malos hábitos te alcanzaron.\nRecuerda: Tu salud es tu mejor inversión.",
                    fontSize = 16.sp,
                    textAlign = TextAlign.Center,
                    color = Color.DarkGray
                )

                // PUNTUACIÓN FINAL
                /**
                 * Mostramos los puntos obtenidos.
                 * 
                 * GAMIFICACIÓN:
                 * Ver tu puntuación motiva a intentar superarla en el siguiente intento.
                 */
                Box(
                    modifier = Modifier
                        .background(
                            color = Color(0xFF1976D2),  // Azul UTNG
                            shape = RoundedCornerShape(8.dp)
                        )
                        .padding(horizontal = 24.dp, vertical = 12.dp)
                ) {
                    Text(
                        text = "Puntuación: $score",
                        fontSize = 24.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color.White
                    )
                }

                // CONSEJOS DE SALUD
                /**
                 * Mensaje educativo sobre hábitos saludables.
                 * 
                 * PROPÓSITO:
                 * Aprovechar el "momento de reflexión" del Game Over
                 * para reforzar el aprendizaje.
                 */
                Text(
                    text = "💡 Consejo: Come bien, hidrátate, y evita sustancias nocivas.",
                    fontSize = 14.sp,
                    textAlign = TextAlign.Center,
                    color = Color(0xFF388E3C)  // Verde esperanza
                )

                Spacer(modifier = Modifier.height(8.dp))

                // BOTÓN REINTENTAR
                /**
                 * Button es el componente de Material Design para botones.
                 * 
                 * CALLBACK: onRestart es una función que se pasa como parámetro.
                 * Cuando el usuario presiona el botón, se ejecuta esta función.
                 */
                Button(
                    onClick = onRestart,  // Ejecutar el callback
                    colors = ButtonDefaults.buttonColors(
                        containerColor = Color(0xFF4CAF50)  // Verde "Go"
                    ),
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(56.dp)
                ) {
                    Text(
                        text = "🔄 Reintentar",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                }
            }
        }
    }
}
```

**Explicación Detallada:**

**¿Qué es un Callback?**

Un callback es una función que se pasa como parámetro para ser ejecutada después.

```kotlin
// Definición del GameOverDialog
fun GameOverDialog(onRestart: () -> Unit)

// Uso en GameScreen
GameOverDialog(
    onRestart = { viewModel.restartGame() }  // Este es el callback
)

// Cuando se presiona el botón en GameOverDialog:
Button(onClick = onRestart)  // Se ejecuta viewModel.restartGame()
```

**Analogía:**
Es como dejar una nota: "Cuando termines de lavar los platos, llámame".
La nota (callback) dice qué hacer, pero no lo hace hasta que llegue el momento.

---

### 4️⃣ MainActivity

#### 📄 `MainActivity.kt`

El punto de entrada de la aplicación.

```kotlin
package com.utng.runner

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import androidx.lifecycle.viewmodel.compose.viewModel
import com.utng.runner.presentation.GameScreen
import com.utng.runner.presentation.GameViewModel

/**
 * MainActivity es la actividad principal de la aplicación.
 * 
 * CONCEPTO: Activity
 * Una Activity es una pantalla en Android.
 * Es el punto de entrada de tu app.
 * 
 * ANALOGÍA: Es como la puerta principal de una casa.
 * Todo el que entra a tu app, entra por aquí.
 * 
 * ComponentActivity es la clase base para apps con Jetpack Compose.
 */
class MainActivity : ComponentActivity() {
    
    /**
     * onCreate es el método que se ejecuta cuando la actividad se crea.
     * 
     * CICLO DE VIDA:
     * onCreate() → onStart() → onResume() → (App corriendo)
     * 
     * ANALOGÍA: Es como llegar a una fiesta.
     * - onCreate: Entras y te presentas
     * - onStart: Te quitas el abrigo
     * - onResume: Empiezas a socializar
     * 
     * @param savedInstanceState Estado guardado de ejecuciones anteriores
     */
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        /**
         * enableEdgeToEdge permite usar toda la pantalla,
         * incluyendo las barras de sistema (status bar, navigation bar).
         * 
         * EFECTO: La app se ve moderna y "full screen"
         */
        enableEdgeToEdge()
        
        /**
         * setContent define el contenido UI de la Activity.
         * 
         * CONCEPTO: Compose UI
         * En lugar de usar XML (activity_main.xml), definimos la UI con código.
         * 
         * VENTAJA:
         * - Más fácil de modificar
         * - Menos archivos que mantener
         * - UI reactiva automáticamente
         */
        setContent {
            /**
             * MaterialTheme aplica el diseño Material Design 3.
             * 
             * Material Design es el sistema de diseño de Google.
             * Define colores, formas, tipografías consistentes.
             * 
             * ANALOGÍA: Es como usar una plantilla de diseño profesional
             * en PowerPoint. Todo se ve coherente automáticamente.
             */
            MaterialTheme {
                /**
                 * Surface es un contenedor básico con color de fondo.
                 * 
                 * PROPÓSITO:
                 * Proporciona un fondo coherente con el tema de Material
                 */
                Surface(
                    modifier = Modifier.fillMaxSize(),  // Ocupa toda la pantalla
                    color = MaterialTheme.colorScheme.background  // Color del tema
                ) {
                    /**
                     * viewModel() crea o recupera el ViewModel.
                     * 
                     * CONCEPTO: ViewModel Lifecycle
                     * El ViewModel sobrevive a rotaciones de pantalla.
                     * 
                     * EJEMPLO:
                     * 1. Usuario está jugando (puntaje = 500)
                     * 2. Usuario rota el teléfono
                     * 3. Activity se destruye y recrea
                     * 4. ViewModel sigue vivo con puntaje = 500
                     * 
                     * Sin ViewModel, perderías el progreso al rotar.
                     */
                    val viewModel: GameViewModel = viewModel()
                    
                    /**
                     * GameScreen es nuestra pantalla de juego.
                     * Le pasamos el ViewModel para que pueda controlar el juego.
                     */
                    GameScreen(viewModel = viewModel)
                }
            }
        }
    }
}
```

**Explicación Detallada:**

**Ciclo de Vida de una Activity:**

```
Usuario abre app
    ↓
onCreate() ──────────┐
    ↓                │
onStart()            │ VISIBLE
    ↓                │ pero no interactuable
onResume() ──────────┘
    ↓
(App corriendo y interactuable)
    ↓
Usuario presiona Home
    ↓
onPause() ───────────┐
    ↓                │ NO VISIBLE
onStop() ────────────┘
    ↓
Usuario regresa a la app
    ↓
onRestart()
    ↓
onStart()
    ↓
onResume()
```

**¿Por qué usar setContent en lugar de XML?**

**Antes (XML):**
```xml
<!-- activity_main.xml -->
<LinearLayout>
    <TextView android:text="Hola" />
    <Button android:text="Click" />
</LinearLayout>
```
```kotlin
// MainActivity.kt
setContentView(R.layout.activity_main)
val button = findViewById<Button>(R.id.button)
```

**Ahora (Compose):**
```kotlin
setContent {
    Column {
        Text("Hola")
        Button(onClick = {}) { Text("Click") }
    }
}
```

**Ventajas:**
- Todo en un lugar (no hay XML separado)
- La UI se actualiza automáticamente cuando los datos cambian
- Menos código boilerplate

---

## 🎮 Cómo Funciona el Juego

### Flujo Completo de Ejecución

Vamos a seguir el viaje de un "frame" (fotograma) del juego, desde que arranca hasta que dibuja en pantalla:

#### 1️⃣ **Inicio de la Aplicación**

```
Usuario abre la app
    ↓
MainActivity.onCreate()
    ↓
setContent crea la UI
    ↓
GameScreen se dibuja por primera vez
    ↓
LaunchedEffect llama viewModel.startGame()
```

#### 2️⃣ **Game Loop (60 veces por segundo)**

```
viewModel.startGame()
    ↓
while (isGameLoopRunning):
    ├─ GameEngine.updateGameState(estadoActual)
    │   ├─ Mover obstáculos hacia la izquierda
    │   ├─ Eliminar obstáculos fuera de pantalla
    │   ├─ Generar nuevos obstáculos
    │   ├─ Aplicar física de salto
    │   ├─ Detectar colisiones
    │   ├─ Actualizar puntuación
    │   └─ Aumentar velocidad
    │
    ├─ _gameState.value = nuevoEstado (emit)
    │   ↓
    ├─ GameScreen detecta cambio de estado
    │   ↓
    ├─ Recomposition (redibuja solo lo que cambió)
    │   ├─ PlayerCharacter actualiza posición
    │   ├─ Obstacles actualizan posición
    │   └─ Text actualiza puntuación
    │
    └─ delay(16ms) para mantener 60 FPS
```

#### 3️⃣ **Interacción del Usuario**

```
Usuario toca la pantalla
    ↓
detectTapGestures detecta el toque
    ↓
Llama viewModel.onJump()
    ↓
GameEngine.jump() crea nuevo estado con isJumping=true
    ↓
_gameState.value = nuevoEstado
    ↓
GameScreen se recompone
    ↓
PlayerCharacter se dibuja con yOffset cambiando (animación de salto)
```

#### 4️⃣ **Detección de Colisión**

```
En updateGameState():
    ↓
detectCollision(playerY, obstacles)
    ├─ Para cada obstáculo:
    │   ├─ Calcular hitbox del jugador
    │   ├─ Calcular hitbox del obstáculo
    │   └─ ¿Se superponen? → COLISIÓN
    │
    └─ Si hay colisión:
        ├─ isGameOver = true
        ├─ Detener el game loop
        └─ Mostrar GameOverDialog
```

### Diagrama de Flujo de Datos

```
┌─────────────┐
│   Usuario   │ (Toca pantalla)
└──────┬──────┘
       │
       ▼
┌─────────────────┐
│   GameScreen    │ (Detecta toque)
│  (UI Layer)     │
└──────┬──────────┘
       │ viewModel.onJump()
       ▼
┌─────────────────┐
│  GameViewModel  │ (Coordinador)
│  (Presentation) │
└──────┬──────────┘
       │ GameEngine.jump()
       ▼
┌─────────────────┐
│   GameEngine    │ (Lógica pura)
│   (Domain)      │
└──────┬──────────┘
       │ Nuevo GameState
       ▼
┌─────────────────┐
│   GameState     │ (Datos)
│   (Data)        │
└──────┬──────────┘
       │ StateFlow emission
       ▼
┌─────────────────┐
│   GameScreen    │ (Observa cambios)
│  (Recompose)    │
└─────────────────┘
```

---

## ✅ Buenas Prácticas Aplicadas

### 1. **Arquitectura en Capas (Layered Architecture)**

**¿Qué es?**
Separar el código en capas con responsabilidades específicas.

**¿Por qué es bueno?**
- **Mantenibilidad**: Fácil encontrar y modificar código
- **Testabilidad**: Puedes probar cada capa independientemente
- **Escalabilidad**: Fácil añadir funcionalidades
- **Reusabilidad**: Las capas pueden reutilizarse en otros proyectos

**Ejemplo de la Vida Real:**
Es como una empresa:
- **Data**: El almacén (solo guarda cosas)
- **Domain**: Los ingenieros (procesan y crean)
- **Presentation**: Los vendedores (presentan al cliente)

### 2. **MVVM (Model-View-ViewModel)**

**¿Qué es?**
Un patrón de arquitectura que separa la lógica de la UI.

**Ventajas:**
- **Separation of Concerns**: Cada parte hace solo su trabajo
- **Testeable**: Puedes probar el ViewModel sin UI
- **Reactive**: La UI se actualiza automáticamente

**Analogía:**
```
Model (Datos) = Receta de cocina
View (UI) = Plato servido
ViewModel = Chef que coordina

Si cambias el chef, la receta y el plato siguen funcionando.
```

### 3. **Inmutabilidad (Immutability)**

**¿Qué es?**
Los objetos no se modifican, se crean copias con los cambios.

```kotlin
// ❌ MALO: Mutación directa
gameState.score = gameState.score + 1

// ✅ BUENO: Inmutabilidad
gameState = gameState.copy(score = gameState.score + 1)
```

**Ventajas:**
- **Seguridad**: No puedes cambiar accidentalmente datos
- **Predecibilidad**: Sabes exactamente qué datos tienes
- **Debugging**: Fácil rastrear cambios

**Analogía:**
En lugar de borrar y reescribir una página, creas una nueva página con los cambios. Así siempre tienes el historial.

### 4. **Single Responsibility Principle (SRP)**

**¿Qué es?**
Cada clase debe tener una sola responsabilidad.

**Ejemplos en nuestro código:**
- `GameEngine`: SOLO lógica del juego
- `GameViewModel`: SOLO coordinar entre UI y lógica
- `GameScreen`: SOLO mostrar UI
- `ObstacleType`: SOLO definir tipos de obstáculos

**Analogía:**
En un restaurante:
- El chef cocina (no limpia mesas)
- El mesero sirve (no cocina)
- El cajero cobra (no cocina ni sirve)

### 5. **Dependency Injection (Inyección de Dependencias)**

**¿Qué es?**
Pasar las dependencias como parámetros en lugar de crearlas dentro.

```kotlin
// ✅ BUENO: ViewModel recibido por parámetro
@Composable
fun GameScreen(viewModel: GameViewModel)

// ❌ MALO: ViewModel creado dentro
@Composable
fun GameScreen() {
    val viewModel = GameViewModel()  // Acoplado
}
```

**Ventajas:**
- **Testeable**: Puedes pasar un mock en tests
- **Flexible**: Fácil cambiar la implementación
- **Reusable**: El componente funciona con cualquier implementación

### 6. **Stateless Composables**

**¿Qué es?**
Componentes UI que no tienen estado propio.

```kotlin
// ✅ BUENO: Stateless
@Composable
fun PlayerCharacter(yOffset: Float) { ... }

// ❌ MALO: Stateful
@Composable
fun PlayerCharacter() {
    var yOffset by remember { mutableStateOf(0f) }  // Estado interno
    ...
}
```

**Ventajas:**
- **Predecible**: Mismos parámetros = misma UI
- **Testeable**: Fácil verificar resultados
- **Reusable**: Funciona en cualquier contexto

### 7. **Naming Conventions (Convenciones de Nombres)**

**Nombres claros y descriptivos:**

```kotlin
// ✅ BUENO
fun detectCollision()
val MAX_JUMP_HEIGHT
class GameEngine

// ❌ MALO
fun dc()
val MJH
class GE
```

**Reglas aplicadas:**
- **Clases**: PascalCase (`GameState`)
- **Funciones/Variables**: camelCase (`updateGameState`)
- **Constantes**: UPPER_SNAKE_CASE (`MAX_JUMP_HEIGHT`)
- **Booleanos**: is/has/should prefix (`isGameOver`)

### 8. **Documentación (KDoc)**

**Comentarios útiles en todo el código:**

```kotlin
/**
 * Descripción de qué hace
 * 
 * @param parametro Descripción del parámetro
 * @return Qué devuelve
 */
fun funcion(parametro: Tipo): Retorno
```

**Tipos de comentarios:**
- **¿Qué hace?**: Describe la función/clase
- **¿Por qué existe?**: Explica el propósito
- **¿Cómo funciona?**: Detalles de implementación
- **Ejemplos**: Casos de uso

### 9. **Constants en Companion Objects**

**Centralizar valores constantes:**

```kotlin
companion object {
    const val MAX_JUMP_HEIGHT = 200f
    const val GRAVITY = 0.8f
}
```

**Ventajas:**
- **DRY**: No repetir valores mágicos
- **Mantenibilidad**: Cambiar en un solo lugar
- **Legibilidad**: Nombres descriptivos

### 10. **Sealed Classes para Estados Finitos**

**Para representar opciones limitadas:**

```kotlin
sealed class ObstacleType { ... }
```

**Ventajas:**
- **Exhaustividad**: El compilador verifica todos los casos
- **Seguridad**: No puedes crear tipos inválidos
- **Autocompletado**: El IDE sugiere todas las opciones

---

## 🚀 Instrucciones de Uso

### Paso 1: Crear el Proyecto en Android Studio

1. Abre Android Studio
2. **File → New → New Project**
3. Selecciona **Empty Activity**
4. Configura:
   - **Name**: UTNG Runner
   - **Package name**: com.utng.runner
   - **Language**: Kotlin
   - **Minimum SDK**: API 24 (Android 7.0)
5. Click en **Finish**

### Paso 2: Configurar build.gradle.kts

Copia el código de configuración del inicio de este manual.

### Paso 3: Crear la Estructura de Carpetas

En `app/src/main/java/com/utng/runner/`:

```
Botón derecho → New → Package:
- data
- domain
- presentation
- presentation.components
```

### Paso 4: Crear los Archivos

Copia cada archivo del manual en su ubicación correspondiente.

**Orden recomendado:**
1. `data/ObstacleType.kt`
2. `data/GameState.kt`
3. `domain/GameEngine.kt`
4. `presentation/GameViewModel.kt`
5. `presentation/components/PlayerCharacter.kt`
6. `presentation/components/Obstacle.kt`
7. `presentation/components/GameOverDialog.kt`
8. `presentation/components/GameScreen.kt`
9. `MainActivity.kt`

### Paso 5: Sincronizar y Ejecutar

1. **Sync Project with Gradle Files** (icono de elefante)
2. Espera a que descargue dependencias
3. Conecta un dispositivo o inicia un emulador
4. Click en **Run (▶️)**

---

## 📱 Capturas Esperadas

**Pantalla Inicial:**
```
╔════════════════════════════╗
║  UTNG Runner               ║  ← Título
║  Toca para saltar...       ║  ← Instrucciones
║                            ║
║  🧑‍🎓                      ║  ← Jugador
║  ━━━━━━━━━━━━━━━━━━━━━━━  ║  ← Suelo
║           🍔               ║  ← Obstáculo
║                            ║
║             Puntos: 150    ║  ← HUD
╚════════════════════════════╝
```

**Game Over:**
```
╔════════════════════════════╗
║    ┌─────────────────┐    ║
║    │  ¡Game Over!    │    ║
║    │      😢         │    ║
║    │ Los malos       │    ║
║    │ hábitos te      │    ║
║    │ alcanzaron      │    ║
║    │                 │    ║
║    │ Puntuación: 350 │    ║
║    │                 │    ║
║    │ [🔄 Reintentar] │    ║
║    └─────────────────┘    ║
╚════════════════════════════╝
```

---

## 🎓 Conceptos Aprendidos

Al completar este proyecto, tus alumnos habrán aprendido:

### Conceptos de Programación:
1. ✅ Arquitectura en capas
2. ✅ MVVM Pattern
3. ✅ Inmutabilidad
4. ✅ Sealed Classes
5. ✅ Data Classes
6. ✅ Companion Objects
7. ✅ Coroutines y Flow
8. ✅ State Management

### Conceptos de Android:
9. ✅ Jetpack Compose
10. ✅ ViewModel
11. ✅ StateFlow
12. ✅ Composables
13. ✅ Recomposition
14. ✅ Lifecycle
15. ✅ Material Design 3

### Conceptos de Juegos:
16. ✅ Game Loop
17. ✅ Collision Detection
18. ✅ Physics Simulation
19. ✅ Frame Rate (FPS)
20. ✅ State Management

### Buenas Prácticas:
21. ✅ Clean Code
22. ✅ SOLID Principles
23. ✅ Documentation
24. ✅ Naming Conventions
25. ✅ Code Organization

---

## 🔧 Posibles Mejoras (Ejercicios)

Sugiere estos ejercicios a tus alumnos:

### Nivel Básico:
1. **Cambiar colores**: Personalizar el tema de colores
2. **Nuevos emojis**: Cambiar el jugador y obstáculos
3. **Velocidad inicial**: Modificar `gameSpeed` base
4. **Gravedad**: Ajustar `GRAVITY` para saltos más altos/bajos

### Nivel Intermedio:
5. **Power-ups**: Añadir ítems buenos (💪, 🥗, 💧)
6. **Niveles**: Incrementar dificultad cada 1000 puntos
7. **Sonidos**: Añadir efectos de salto y colisión
8. **Vibración**: Vibrar al perder
9. **Récord**: Guardar el mejor puntaje

### Nivel Avanzado:
10. **Multiplayer local**: Dos jugadores en la misma pantalla
11. **Leaderboard**: Tabla de mejores puntajes con nombres
12. **Achievements**: Sistema de logros
13. **Temas**: Modo día/noche, diferentes escenarios
14. **Animaciones**: Transiciones suaves y efectos visuales

---

## 📚 Recursos Adicionales

### Documentación Oficial:
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel)
- [Kotlin Coroutines](https://kotlinlang.org/docs/coroutines-overview.html)
- [Material Design 3](https://m3.material.io/)

### Tutoriales Recomendados:
- [Compose Basics](https://developer.android.com/courses/pathways/compose)
- [MVVM Architecture](https://developer.android.com/topic/architecture)
- [Game Development in Compose](https://developer.android.com/codelabs/basic-android-kotlin-compose-game)

---

## 💡 Consejos para el Docente

### Para explicar el proyecto:
1. **Empieza por la UI**: Muestra el resultado final primero
2. **Luego la arquitectura**: Explica las capas
3. **Después el flujo**: Sigue un frame del juego
4. **Finalmente detalles**: Profundiza en cada archivo

### Para ejercicios:
- Comienza con modificaciones simples (cambiar colores)
- Incrementa complejidad gradualmente
- Fomenta la experimentación
- Revisa código en clase

### Para evaluación:
- **30%**: Implementación correcta del código
- **25%**: Comprensión de arquitectura
- **20%**: Documentación y comentarios
- **15%**: Creatividad en mejoras
- **10%**: Presentación del proyecto

---

## 🎉 Conclusión

Este proyecto combina:
- 📱 Desarrollo móvil moderno con Jetpack Compose
- 🎮 Programación de juegos con física y colisiones
- 🏗️ Arquitectura profesional con MVVM
- 💊 Mensaje social sobre salud y bienestar

**Mensaje Final para los Estudiantes:**

Así como en este juego el alumno de la UTNG debe evitar malos hábitos para seguir adelante, en la vida real también enfrentarás decisiones cada día. Tu futuro profesional depende de las decisiones que tomes hoy:

- **🍔 Mala alimentación** = Bajo rendimiento académico
- **🍺 Alcohol** = Pérdida de oportunidades
- **💊 Drogas** = Destrucción de sueños y metas

¡Mantén buenos hábitos, estudia, cuídate y alcanza tus metas! 🎓✨

**¡Feliz Codificación!** 👨‍💻👩‍💻

---

## 📞 Soporte

Si tienen dudas sobre el proyecto:
1. Revisen los comentarios en el código
2. Consulten la documentación oficial de Android
3. Pregunten a su profesor
4. Experimenten y aprendan haciendo

---

**Versión:** 1.0  
**Fecha:** Noviembre 2025  
**Autor:** Docente UTNG  
**Propósito:** Material educativo para curso de Desarrollo Móvil

---

*Este proyecto fue creado con fines educativos para enseñar desarrollo Android con Jetpack Compose y promover hábitos saludables entre los estudiantes universitarios.*
