# Estándar de Coherencia y Auditoría Interna · Equipo E3

| Metadato | Detalle |
|---|---|
| **Documento** | Contrato Terminológico y Matriz de Trazabilidad Cruzada |
| **Equipo / Módulo** | E3 · Análisis — Arena de combate (RPG Manager) |
| **Responsable** | Cristian Andrés Olivero Garzón (Trazabilidad y Coherencia) |
| **Estado** | **Estándar activo** (Auditoría de archivos del squad en curso) |

---

## 1. Estándar Terminológico Obligatorio (Cero Disonancia entre Artefactos)

Para evitar penalizaciones en la rúbrica por inconsistencia de nombres entre los artefactos **8 (Modelo del Dominio)**, **9 (Diagrama de Actividades)** y **10 (Prototipo de Interfaz)**, todo el equipo debe respetar estrictamente esta tabla:

| Concepto Oficial | Nombre Exacto Obligatorio | Términos Prohibidos | Uso en los Artefactos de E3 |
|---|---|---|---|
| **Arena** | `Arena` / `Arena de Combate` | Sala, Coliseo Room, Tablero | Entidad raíz en Art. 8 y título en Pantalla 1 (Art. 10). |
| **Combate** | `Combate` | Pelea, Duelo, Battle, Match | Entidad central en Art. 8 y proceso principal en Art. 9. |
| **Personaje** | `Personaje` (roles: `Jugador` / `Rival`) | Héroe, Avatar, Monstruo, Bot | Entidad en Art. 8, carril `Jugador` en Art. 9 y tarjetas en Art. 10. |
| **Turno** | `Turno` | Ronda, Ciclo, Step | Entidad en Art. 8, bucle en Art. 9 y encabezado en Pantalla 2 (Art. 10). |
| **Ataque** | `Ataque` | Golpe, Punch, Básico | Especialización en Art. 8, rama en Art. 9 y botón en Pantalla 2 (Art. 10). |
| **Habilidad** | `Habilidad` | Magia, Spell, Poder | Especialización en Art. 8, rama en Art. 9 y botón en Pantalla 2 (Art. 10). |
| **Resultado** | `Resultado` (`Victoria` / `Derrota`) | Game Over, Score, Fin | Entidad final en Art. 8, cierre en Art. 9 y Pantalla 3 (Art. 10). |
| **Caso de Uso** | `CU-07 Combatir en la Arena` | CU-1, Pelear, Combate | Etiqueta obligatoria anotada en las 3 pantallas del Prototipo (Art. 10). |

---

## 2. Matriz de Trazabilidad Elemento a Elemento

| Requisito / CU | Modelo del Dominio (Art. 8) | Diagrama de Actividades (Art. 9) | Prototipo de Interfaz (Art. 10) | Proyección E4 / E5 / E6 |
|---|---|---|---|---|
| **RF-ARENA-01**<br>(`CU-07` Paso 1) | `Arena`, `Personaje`, `Combate` | Carril Jugador: Seleccionar rival.<br>Carril Sistema: Decisión `¿vida > 0?`. | **Pantalla 1: Selección de Rival**<br>*(Anotar: Soporta `CU-07`)* | **E4:** `CombateService.iniciarCombate()`<br>**E5:** Tabla `combate` (`id_combate`)<br>**E6:** `CP-ARENA-01` (Rival válido) |
| **RF-ARENA-02**<br>(`CU-07` Pasos 2-4) | `Turno`, `Ataque`, `Habilidad` | Carril Jugador: Elegir Ataque/Habilidad.<br>Carril Sistema: Decisión `¿esquiva el ataque?`. | **Pantalla 2: Turno de Combate**<br>*(Anotar: Soporta `CU-07`)* | **E4:** `Personaje.intentarEsquivar()`<br>**E5:** Tabla `turno_combate`<br>**E6:** `CP-ARENA-02` (Cálculo esquiva) |
| **RF-ARENA-03**<br>(`CU-07` Alt.) | `Personaje`, `Turno` | Carril Jugador: Usar poción.<br>Carril Sistema: Restaurar vida y gastar turno. | **Pantalla 2: Turno de Combate**<br>(Botón *Usar Poción*) | **E4:** `InventarioService.consumirPocion()`<br>**E6:** `CP-ARENA-03` (Uso de ítem) |
| **RF-ARENA-04**<br>(`CU-07` Paso 5) | `Combate`, `Resultado`, `Personaje` | Carril Sistema: Decisión `¿vida <= 0?` (rompe ciclo) y generar resultado. | **Pantalla 3: Resultado**<br>*(Anotar: Soporta `CU-07`)* | **E4:** `Resultado.generarReporte()`<br>**E5:** Tabla `resultado_combate`<br>**E6:** `CP-ARENA-04` (Victoria/Derrota) |
| **RNF-ARENA-01 / 02** | Invariante conceptual: `vida >= 0` | Guarda: si `daño > vida`, fijar `vida = 0`. | Barra de HP nunca muestra valores `< 0`. | **E5:** `CHECK (vida >= 0)` en MariaDB<br>**E6:** `CP-ARENA-05` (Tiempo `<= 1.5s`) |

---

## 3. Checklist de Auditoría Cruzada (Verificación Antes de Exponer)

> *Instrucciones para el equipo: Marcar con `[x]` únicamente cuando el archivo real esté subido al repositorio y cumpla la regla.*

### Artefacto 8: Modelo del Dominio (`01_modelo_dominio.puml` + imagen)
- [ ] Contiene los 7 conceptos mínimos: `Arena`, `Combate`, `Turno`, `Personaje`, `Ataque`, `Habilidad`, `Resultado`.
- [ ] **Regla de oro:** No incluye tipos de dato (`int`, `String`), ni visibilidad (`+`, `-`), ni métodos de software.
- [ ] Incluye asociaciones nombradas y multiplicidades claras.
- [ ] Incluye una `note` dentro del diagrama explicando en qué se diferencia del Diagrama de Clases de Diseño (E4).

### Artefacto 9: Diagrama de Actividades (`02_diagrama_actividades.puml` + imagen)
- [ ] Usa carriles (particiones) explícitos: `|Jugador|` y `|Sistema|`.
- [ ] Tiene nodo inicial, flujo cíclico de turnos (`repeat / while`) y nodo final.
- [ ] Contiene al menos las decisiones obligatorias: `¿esquiva el ataque?` y `¿vida <= 0?`.

### Artefacto 10: Prototipo de Interfaz (`03_prototipo_interfaz...`)
- [ ] Contiene las 3 pantallas exigidas: **Selección de rival**, **Turno de combate** y **Resultado**.
- [ ] Muestra flechas o flujo claro de navegación entre las 3 pantallas.
- [ ] Tiene anotado explícitamente en cada pantalla que soporta el caso de uso **`CU-07 Combatir en la Arena`**.

### Entregables Generales de Carpeta (`rup/E3-arena-de-combate/`)
- [ ] `README.md` actualizado con el dueño responsable de cada artefacto.
- [ ] `preguntas_dinamica.md` con 3 a 5 preguntas (incluyendo *"¿qué artefacto usarías?"* y *"encuentra el error"*).
- [ ] `presentacion.pdf` con máximo 8 diapositivas enfocadas en los ejemplos de Arena de Combate.
