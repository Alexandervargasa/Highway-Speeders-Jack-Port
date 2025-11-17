# Highway Speeders - Jack Language Implementation

![Highway Speeders](https://img.shields.io/badge/Language-Jack-orange)
![Platform](https://img.shields.io/badge/Platform-Nand2Tetris-blue)
![Genre](https://img.shields.io/badge/Genre-Racing-red)

## Descripción

Highway Speeders es una implementación del clásico juego de carreras Road Fighter, desarrollado en lenguaje Jack para la plataforma educativa Nand2Tetris. El jugador conduce un vehículo a alta velocidad por una autopista, esquivando tráfico enemigo, gestionando su combustible y utilizando nitro para alcanzar velocidades extremas.

## Desarrolladores

- **Simon Martinez Gomez**
- **Jacobo Pava**
- **Alexander Vargas**

## Características del Juego

### Mecánicas de Juego

- **Sistema de conducción**: Movimiento lateral fluido con aceleración y frenado
- **Tráfico dinámico**: Vehículos enemigos con spawn procedural en 3 carriles
- **Sistema de combustible**: Gestión de recursos que afecta la duración del juego
- **Modo Nitro**: Boost de velocidad con consumo triplicado de combustible
- **Power-ups**: Bidones de combustible con spawn aleatorio para recargar
- **Puntuación dinámica**: Sistema de scoring basado en distancia recorrida y vehículos adelantados
- **Interfaz completa**: Menú principal, HUD en tiempo real y pantalla de Game Over

### Controles

- **Flecha Izquierda (←)**: Moverse al carril izquierdo
- **Flecha Derecha (→)**: Moverse al carril derecho
- **Flecha Arriba (↑)**: Acelerar (aumenta velocidad base)
- **Flecha Abajo (↓)**: Frenar (reduce velocidad base)
- **SPACE**: Activar Nitro Boost (velocidad máxima con consumo x3)
- **ESC**: Salir del juego
- **BACKSPACE**: Volver al Menu (en pantallad de game over)

### Sistema de Puntuación

- **Distancia recorrida**: Puntos por segundo según velocidad (velocidad × 3)
- **Adelantamientos**: 20 puntos por cada vehículo que sale de pantalla
- **Recolección de combustible**: 50 puntos bonus por cada power-up

### Condiciones de Game Over

- **Sin combustible**: El tanque llega a 0
- **Colisión**: Choque con vehículo enemigo

## Estructura del Proyecto
```
HighwaySpeeders/
│
├── Main.jack            # Loop principal y gestión de estados del juego
├── GameState.jack       # Sistema de menús y pantallas (MENU/PLAYING/GAMEOVER)
├── Player.jack          # Vehículo del jugador con sistema de nitro
├── Enemy.jack           # Clase de vehículos enemigos individuales
├── EnemyManager.jack    # Pool de enemigos y sistema de spawn
├── Road.jack            # Carretera con efecto de scroll infinito
├── FuelPowerup.jack     # Power-ups de combustible recolectables
├── GameGUI.jack         # Interfaz de usuario (HUD con tiempo, fuel y score)
└── README.md            # Este archivo
```

## Cómo Compilar y Ejecutar

### Usando el IDE Online de Nand2Tetris

1. **Accede al Jack Compiler:**
   - Ingresa al IDE de Nand2Tetris y navega a la sección del compilador Jack

2. **Compila el proyecto:**
   - Haz clic en el ícono de carpeta junto a **"Source"**
   - Localiza y selecciona el directorio `Highway Speeders`
   - Presiona el botón `Compile` - se generarán automáticamente los archivos `.vm`

3. **Ejecuta en el VMEmulator:**
   - Después de compilar, abrir el VMEmulator (necesariamente descargar las tools de la pagina de Nand2tetris), selecionar el directorio de `Highway Speeders`
   - Ajusta la velocidad a "Fast" para una experiencia óptima (recomendado)
   - Presiona **Run** y disfruta del juego

### Recomendaciones de Ejecución

- **Velocidad**: Configurar en "Fast" para 50ms de delay por frame
- **Pantalla**: Maximizar la ventana del emulador para mejor visibilidad
- **Audio**: El juego no incluye sonido (limitación de la plataforma)

## Características Técnicas

### Arquitectura del Sistema

- **Máquina de estados**: Implementación de FSM (Finite State Machine) para flujo del juego
- **Object Pooling**: Reciclaje de enemigos para optimización de memoria
- **Pseudo-aleatoriedad**: Sistema de generación procedural usando variables del juego
- **Rendering selectivo**: Actualización solo de áreas cambiantes para mejor performance

### Sistemas Implementados

1. **Sistema de Física**
   - Velocidad relativa entre jugador y enemigos
   - Perspectiva simulada mediante posición Y variable
   - Scroll infinito de carretera con offset cíclico

2. **Sistema de Colisiones**
   - AABB (Axis-Aligned Bounding Box) para detección de impactos
   - Verificación eficiente solo con objetos activos
   - Colisión con power-ups para recolección

3. **Sistema de Combustible**
   - Consumo variable según velocidad (1-3 unidades/seg)
   - Multiplicador x3 durante modo nitro
   - Power-ups con spawn procedural cada 8-12 segundos

4. **Sistema de Scoring**
   - Puntos por distancia en tiempo real
   - Tracking de adelantamientos sin duplicados
   - Protección contra overflow de enteros (límite 30,000)

### Optimizaciones

- **Clipping de renderizado**: Objetos fuera de pantalla no se dibujan
- **Actualización parcial del GUI**: Solo se redibuja lo que cambia
- **Gestión eficiente de memoria**: Dispose correcto de objetos entre estados
- **Rate limiting**: Spawn de power-ups con intervalos variables

### Limitaciones de la Plataforma

- **Resolución**: 512×256 píxeles en blanco y negro
- **Sin sprites**: Gráficos basados en primitivas geométricas (rectángulos)
- **Performance**: Velocidad limitada por el VMEmulator (~20 FPS)
- **Sin persistencia**: No hay almacenamiento de high scores

## Diseño Visual

### HUD (Heads-Up Display)
```
[Barra Speed]  TIME: 45    FUEL: [████████] SCORE: 1250
[NITRO!]       ────────────────────────────────────────
```

### Pantalla de Juego
- **Carretera**: 220 píxeles de ancho con líneas divisorias animadas
- **3 Carriles**: Distribución equitativa del espacio
- **Jugador**: Rectángulo negro de 20×30px con ventana
- **Enemigos**: Rectángulos de 18×28px con detalles visuales
- **Power-ups**: Ícono de bidón de 16×16px con letra "F"

## Documentación del Código

Cada archivo `.jack` incluye:
- **Comentarios de clase**: Propósito y responsabilidades
- **Documentación de métodos**: Parámetros, retornos y efectos secundarios
- **Comentarios inline**: Explicación de lógica compleja
- **Manejo de memoria**: Dealloc explícito y gestión de recursos

### Conceptos de Ciencias de la Computación Aplicados

- **Compilación**: De código de alto nivel Jack a VM bytecode
- **Máquina Virtual**: Ejecución de bytecode en el VM de Nand2Tetris
- **POO**: Encapsulación, herencia conceptual y polimorfismo
- **Algoritmos**: Detección de colisiones, generación procedural, FSM
- **Estructuras de datos**: Arrays, objetos y gestión manual de memoria
- **Optimización**: Balance entre funcionalidad y limitaciones de recursos

## Logros Técnicos

- Sistema completo de menús y estados
- Física de conducción fluida
- IA básica de tráfico con comportamiento creíble
- Sistema de recursos (combustible) con balance fino
- Power-ups con spawn procedural no predecible
- Modo nitro con trade-off estratégico
- Score system con múltiples fuentes de puntos
- GUI actualizado en tiempo real sin flickering

## Contexto Académico

Este proyecto fue desarrollado como parte del curso de Organizacion de Computadores, de la Universidad EAFIT, utilizando la plataforma Nand2Tetris para comprender:
- El stack completo de software (desde hardware hasta aplicaciones)
- Diseño de lenguajes de programación
- Funcionamiento interno de compiladores y VMs
- Gestión de memoria a bajo nivel
- Limitaciones y trade-offs en sistemas embebidos

---

**Highway Speeders** - Una experiencia de carreras clásica implementada desde los fundamentos de la computación

## Video de la sustentacion
