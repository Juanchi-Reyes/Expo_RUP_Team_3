# Eslabón de Trazabilidad · Equipo E3: Arena de Combate

| Metadato | Detalle |
|---|---|
| **Equipo / Bloque RUP** | E3 · Análisis (Fase de Elaboración) |
| **Módulo RPG Manager** | Arena de combate |
| **Roles RUP** | Analista de Sistemas + Diseñador de Interfaz |
| **Pareja de Auditoría** | E4 (Misiones y progresión) |

> **Nota de exposición:** *"En un proyecto RUP real a escala completa, la Visión y los Requisitos del módulo de combate habrían sido producidos por los equipos de Inicio (E1) y Requisitos (E2). El Equipo E3 sintetiza aquí el insumo mínimo necesario para fundamentar nuestros 3 artefactos de análisis en la fase de Elaboración."*

---

## 1. Insumo Mínimo Recibido (Fases de Inicio y Requisitos: E1 / E2)

### A. Glosario Oficial del Módulo (Insumo de E1)
* **Personaje:** Entidad controlada por un jugador (o IA rival) con clase, nivel, puntos de vida (HP) y atributos. *(Prohibido usar sinónimos como "Héroe" o "Avatar").*
* **Vida (HP):** Puntos de vitalidad actuales del personaje. Al llegar a `0`, el combate termina.
* **Arena:** Escenario virtual donde se arbitra un combate por turnos bajo reglas formales.
* **Ataque:** Acción ofensiva física básica sin costo de energía.
* **Habilidad:** Acción especial mágica o táctica con efectos específicos.

### B. Requisitos Mínimos del Módulo (Insumo de E2)
* **RF-ARENA-01 (Selección y Desafío):** El sistema debe permitir al Jugador seleccionar su Personaje activo (`vida > 0`) y elegir un Rival disponible en la Arena para iniciar un combate.
* **RF-ARENA-02 (Turnos y Esquiva):** El sistema debe gestionar turnos alternados (Jugador vs. Sistema). En cada turno el atacante elige un `Ataque` o `Habilidad`, y el defensor evalúa su probabilidad de esquivar.
* **RF-ARENA-03 (Uso de Poción):** El sistema debe permitir consumir una poción de vida del inventario durante el turno propio (trazable con `RF-INV-05` de E2).
* **RF-ARENA-04 (Resolución y Resultado):** Si la vida de cualquier contendiente llega a `<= 0`, el sistema finaliza el combate de inmediato, emite un `Resultado` (Victoria/Derrota) y asigna experiencia.
* **RNF-ARENA-01 (Rendimiento):** El cálculo del turno y respuesta visual debe ejecutarse en `<= 1.5 segundos`.
* **RNF-ARENA-02 (Integridad de Vida):** Los puntos de vida nunca pueden almacenarse con valores negativos (límite mínimo `0`).

### C. Caso de Uso Insumo: `CU-07 Combatir en la Arena`
* **Actor:** Jugador | **Precondición:** Personaje con `vida > 0` dentro de la Arena.
* **Flujo Básico (5 pasos):**
  1. El Jugador selecciona un Rival en la Arena e inicia el combate.
  2. El Sistema crea la sesión de `Combate`, fija el orden de `Turno` y muestra el estado inicial.
  3. En su turno, el Jugador elige ejecutar un `Ataque`, usar una `Habilidad` o consumir una poción.
  4. El Sistema valida si el rival esquiva el impacto, calcula el daño, actualiza la vida y ejecuta el turno del rival.
  5. Cuando un contendiente llega a `vida <= 0`, el Sistema cierra el bucle, genera el `Resultado` y otorga recompensas.

---

## 2. Trazabilidad Detallada de los 3 Artefactos del Equipo E3

A continuación se explica para cada artefacto asignado al Equipo E3 **de dónde proviene**, **qué requisitos soporta** y **qué artefactos posteriores dependen de él**:

### 1. Artefacto 8: Modelo del Dominio (`01_modelo_dominio.puml`)
* **¿De dónde proviene?:** Del **Glosario del sistema (E1)** y del análisis conceptual de los sustantivos clave extraídos de los requisitos funcionales del módulo *Arena de combate*.
* **¿Qué requisitos soporta?:** Soporta estructuralmente toda la familia **`RF-ARENA-01` a `RF-ARENA-04`** y **`RNF-ARENA-02`**, estableciendo sin ambigüedad las 7 entidades conceptuales del problema (`Arena`, `Combate`, `Turno`, `Personaje`, `Ataque`, `Habilidad`, `Resultado`) y sus multiplicidades (sin métodos ni tipos de dato).
* **¿Qué artefactos posteriores dependen de él?:**
  * **Artefacto 12 — Diagrama de Clases de Diseño (E4):** Toma estos 7 conceptos como base para convertirlos en clases de software con atributos tipados, visibilidad (`+`/`-`) y métodos.
  * **Artefacto 15 — Modelo de Datos (E5):** Depende de estas entidades y cardinalidades para diseñar las tablas relacionales en MariaDB (`combate`, `turno_combate`, `resultado_combate`).

### 2. Artefacto 9: Diagrama de Actividades (`02_diagrama_actividades.puml`)
* **¿De dónde proviene?:** De la **Especificación del Caso de Uso `CU-07 Combatir en la Arena` (E2)**, traduciendo su flujo básico y flujos alternativos en carriles de responsabilidad (`Jugador` vs. `Sistema`).
* **¿Qué requisitos soporta?:** Soporta directamente **`RF-ARENA-02`** (alternancia de turnos, ciclo repetitivo y decisión `¿esquiva el ataque?`), **`RF-ARENA-03`** (rama de consumo de poción en turno) y **`RF-ARENA-04`** (decisión de ruptura de ciclo `¿vida <= 0?`).
* **¿Qué artefactos posteriores dependen de él?:**
  * **Artefacto 10 — Prototipo de Interfaz (E3):** Toma las transiciones entre carriles para definir el flujo entre pantallas.
  * **Artefacto 13 — Diagrama de Secuencia (E4):** Usa las decisiones y acciones del carril `Sistema` para ordenar las llamadas entre objetos (`CombateService`).
  * **Artefacto 16 — Código Fuente (E5) y Artefacto 17 — Casos de Prueba (E6):** Implementan la lógica de bucle y validan cada camino de decisión (`esquiva = sí/no`, `vida <= 0`).

### 3. Artefacto 10: Prototipo de Interfaz (`Artefacto10_Prototipo_interfaz/`)
* **¿De dónde proviene?:** Del **Caso de Uso `CU-07 Combatir en la Arena` (E2)** y del **Diagrama de Actividades (Art. 9)**, materializando cada interacción del carril `Jugador` en vistas navegables (`P01_seleccion_rival.png`, `P02_turno_combate.png`, `P03_resultado.png` y `navegacion.puml`).
* **¿Qué requisitos soporta?:**
  * `P01_seleccion_rival.png` soporta **`RF-ARENA-01`** (selección de personaje activo y rival válido).
  * `P02_turno_combate.png` soporta **`RF-ARENA-02`**, **`RF-ARENA-03`** y **`RNF-ARENA-01 / 02`** (controles de ataque/habilidad/poción y barras de vida `>= 0`).
  * `P03_resultado.png` soporta **`RF-ARENA-04`** (visualización de victoria/derrota y recompensas).
* **¿Qué artefactos posteriores dependen de él?:**
  * **Artefacto 11 — Documento de Arquitectura SAD (E4):** Define los requerimientos visuales de la capa de presentación (Vista).
  * **Artefacto 20 — Manual de Usuario y Acta de Aceptación (E6):** Utiliza estas pantallas para documentar el uso del módulo ante el cliente y validar los criterios de aceptación.