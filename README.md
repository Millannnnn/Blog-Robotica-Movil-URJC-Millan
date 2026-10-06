# Blog Robotica Movil-URJC-Millan
# Basic Vacuum Cleaner

## Descripción del proyecto

En esta práctica se desarrolla el comportamiento de una **aspiradora autónoma** capaz de desplazarse por una vivienda evitando obstáculos.
El objetivo principal es conseguir que el robot pueda recorrer la mayor superficie posible sin quedarse bloqueado contra paredes, muebles o esquinas.
Para ello se utilizan principalmente:

- Sensores láser.
- Velocidad lineal.
- Velocidad angular.
- Máquina de estados.
- Temporizadores.
- Comportamientos pseudoaleatorios.

# Desarrollo

## 1. Movimiento básico del robot

El primer paso fue aprender a controlar el movimiento del robot.
El movimiento se controla mediante dos variables principales:

### Velocidad lineal
### Velocidad angular

# Máquina de estados
Para organizar el comportamiento del robot se utiliza una máquina de estados.
Se utilizan principalmente tres estados:

## Estado AVANZANDO

Durante este estado el robot se mueve hacia delante.
Mientras avanza, se comprueba continuamente la distancia frontal.
Si el robot detecta un obstáculo demasiado cerca, cambia al estado `RETROCEDIENDO`.

## Estado RETROCEDIENDO

Cuando el robot detecta un obstáculo cercano, retrocede durante un pequeño intervalo.
Para controlar la duración se utiliza el tiempo: tiempo_inicio = time.time()

Posteriormente se comprueba cuánto tiempo ha pasado:
if time.time() - tiempo_inicio >= 1:
    estado = "GIRANDO"

De esta forma no es necesario utilizar sleep(), por lo que el programa sigue ejecutándose continuamente.

## Estado GIRANDO

Después de retroceder, el robot necesita buscar una dirección diferente.La dirección de giro puede decidirse utilizando la información del sensor láser.

Se compara el espacio existente a izquierda y derecha.

# Navegación pseudoaleatoria

Uno de los problemas encontrados durante el desarrollo fue que el robot podía entrar en ciclos repetitivos. Debido a la geometría del entorno, este comportamiento podía repetirse indefinidamente. Para intentar romper estos ciclos se añadió una componente aleatoria. Normalmente el robot gira hacia el lado donde detecta mayor espacio. Pero en algunas ocasiones se fuerza el sentido contrario, esto introduce una pequeña componente pseudoaleatoria en el comportamiento.

# Problemas encontrados

## Robot bloqueado contra una pared

Uno de los primeros problemas fue que el robot podía seguir intentando avanzar aunque tuviera una pared delante. Inicialmente se comparaban únicamente las distancias de diferentes direcciones. Esto no indicaba realmente si existía un obstáculo cercano. La solución fue establecer una distancia mínima frontal.

## Robot atrapado girando

En algunos puntos del mapa el robot podía permanecer demasiado tiempo dentro del estado GIRANDO. Para evitarlo se añadió un tiempo máximo.


Esto permite que el robot vuelva a retroceder y pruebe otra maniobra.

---

## Patrones repetitivos

También se observaron situaciones en las que el robot repetía continuamente los mismos movimientos.

Por ejemplo:

```text
adelante
atrás
giro
adelante
atrás
giro contrario
adelante
atrás
...
```

Para reducir este problema se añadió aleatoriedad al sentido de giro.

---

# 💻 Estructura general del código

La estructura principal del programa es similar a:

```python
while True:

    laser = HAL.getLaserData()

    if len(laser.values) > 0:

        # Leer sensores

        if estado == "AVANZANDO":
            # Avanzar
            # Detectar obstáculos

        elif estado == "RETROCEDIENDO":
            # Retroceder
            # Controlar tiempo

        elif estado == "GIRANDO":
            # Decidir dirección
            # Girar

    Frequency.tick()
```

Esta estructura permite ejecutar continuamente el comportamiento del robot sin bloquear el programa.

---

# 📊 Esquema del funcionamiento

```text
                     ┌───────────────┐
                     │   AVANZANDO   │
                     └───────┬───────┘
                             │
                      obstáculo cerca
                             │
                             ▼
                  ┌─────────────────────┐
                  │   RETROCEDIENDO     │
                  └──────────┬──────────┘
                             │
                       pasa un tiempo
                             │
                             ▼
                     ┌───────────────┐
                     │    GIRANDO    │
                     └───────┬───────┘
                             │
                        espacio libre
                             │
                             └──────────────► AVANZANDO
```

---

# 📸 Resultados

En esta sección se pueden añadir capturas o GIFs mostrando el funcionamiento del robot.

Ejemplo:

```markdown
![Robot funcionando](images/resultado.gif)
```

También puede ser interesante mostrar diferentes situaciones.

### Navegación normal

```markdown
![Navegación](images/navegacion.png)
```

### Detección de obstáculos

```markdown
![Obstáculo](images/obstaculo.png)
```

### Situación de bloqueo

```markdown
![Bloqueo](images/bloqueo.png)
```

---

# 📁 Organización del repositorio

Una posible estructura para el proyecto es:

```text
Basic-Vacuum-Cleaner/
│
├── README.md
│
├── code/
│   └── vacuum_cleaner.py
│
├── images/
│   ├── simulador.png
│   ├── navegacion.png
│   ├── obstaculo.png
│   ├── bloqueo.png
│   └── resultado.gif
│
└── docs/
```

---

# 🧪 Posibles mejoras

Aunque el comportamiento actual permite al robot desplazarse por el entorno, existen diferentes mejoras posibles:

- Ajustar mejor las distancias de detección.
- Optimizar las velocidades de movimiento.
- Mejorar la selección de la dirección de giro.
- Utilizar medias de las regiones del láser para estimar el espacio disponible.
- Detectar situaciones de bloqueo.
- Introducir mayor aleatoriedad en determinados comportamientos.
- Implementar movimientos en espiral.
- Analizar la superficie recorrida.
- Reducir zonas recorridas repetidamente.

---

# 📚 Conceptos aprendidos

Durante esta práctica se han trabajado diferentes conceptos relacionados con programación y robótica:

- Programación básica en Python.
- Bucles `while`.
- Condicionales `if`, `elif` y `else`.
- Listas y rangos.
- Funciones como `min()`.
- Uso de temporizadores con `time`.
- Generación de valores aleatorios.
- Lectura de sensores.
- Control de velocidad lineal y angular.
- Máquinas de estados.
- Navegación reactiva.
- Detección y evasión de obstáculos.
- Depuración de comportamientos robóticos.

---

# ✅ Conclusión

Esta práctica ha permitido desarrollar un sistema básico de navegación autónoma utilizando únicamente información procedente del sensor láser.

A partir de una primera implementación sencilla, se fueron detectando diferentes problemas como colisiones, bloqueos en esquinas y ciclos repetitivos.

Mediante el uso de una máquina de estados, regiones del sensor láser, temporizadores y comportamientos pseudoaleatorios se consiguió desarrollar una estrategia de navegación más robusta.

La práctica también ha servido como introducción al desarrollo de comportamientos reactivos en robots móviles y al uso de Python dentro de un entorno de simulación robótica.

---

## 👨‍💻 Autor

**Nombre:** TU NOMBRE  
**Asignatura:** TU ASIGNATURA  
**Curso:** 2026/2027  
**Proyecto:** Basic Vacuum Cleaner  

---

⭐ *Proyecto desarrollado utilizando Robotics Academy / Unibotics.*
