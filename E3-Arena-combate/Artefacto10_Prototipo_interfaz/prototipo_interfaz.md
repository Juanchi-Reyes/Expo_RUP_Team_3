
# Artefacto 10 - Prototipo de interfaz

## RPG Manager - Arena de combate

| Información | Descripción |
|---|---|
| Proyecto | RPG Manager |
| Equipo | E3 |
| Módulo | Arena de combate |
| Artefacto | Prototipo de interfaz |
| Disciplina RUP | Análisis |
| Fase principal | Elaboración |
| Rol responsable | Diseñador de interfaz |
| Responsable | Juan Nicolás Reyes Saavedra |
| Herramienta | Google Stitch |
| Actor principal | Jugador |
| Versión | 1.0 |

---

## 1. Descripción del artefacto

El prototipo de interfaz representa visualmente la interacción
del jugador con el módulo Arena de combate de RPG Manager.

Permite validar la distribución de los componentes gráficos,
las acciones disponibles y la navegación entre pantallas
antes de iniciar la implementación del software.

## 2. Objetivo

Diseñar una representación visual de la Arena de combate
que permita al jugador seleccionar un rival, ejecutar
acciones durante un combate por turnos y consultar
el resultado final.

## 3. Alcance

El prototipo está compuesto por tres pantallas:

1. Selección de rival.
2. Turno de combate.
3. Resultado del combate.

No incluye la implementación de la lógica del combate,
la persistencia de datos ni el sistema de recompensas.

## 4. Identidad visual

Las interfaces se diseñan para una aplicación web de
escritorio con una temática medieval fantástica.

| Elemento | Valor |
|---|---|
| Fondo principal | #10141F |
| Paneles | #202938 |
| Color principal | #D4AF37 |
| Barra de vida | #DC4040 |
| Barra de maná | #3976D8 |
| Tipografía de títulos | Cinzel |
| Tipografía de contenido | Inter |
| Resolución de referencia | 1440 x 900 px |
| Idioma | Español |

---

## 5. Pantallas del prototipo

### 5.1. P01 - Selección de rival

**Caso de uso:** CU-ARE-01 - Emparejar oponente.

**Actor:** Jugador.

**Objetivo:** Permitir que el jugador seleccione un rival
compatible e inicie un combate.

#### Prototipo

![P01 - Selección de rival](prototipo/P01_seleccion_rival.png)

#### Componentes de la interfaz

| Componente | Funcionalidad |
|---|---|
| Personaje propio | Muestra nombre, clase, nivel, vida y maná. |
| Lista de rivales | Presenta los oponentes compatibles. |
| Tarjeta de rival | Muestra las características del oponente. |
| Botón Seleccionar | Permite elegir un rival. |
| Indicador de compatibilidad | Informa el rango de niveles permitido. |
| Aviso de emparejamiento | Informa sobre la asignación de un rival CPU. |
| Botón Iniciar combate | Navega a la pantalla P02. |
| Botón Volver al menú | Regresa al menú principal. |

#### Validaciones

- El rival debe tener una diferencia máxima de un nivel.
- No se permite iniciar sin un rival seleccionado o asignado.
- Si no se encuentra un rival en 30 segundos, se asigna
  un oponente CPU.

#### Navegación

- Iniciar combate → P02.
- Volver al menú → Menú principal.

---

### 5.2. P02 - Turno de combate

**Casos de uso:**

- CU-ARE-02 - Ejecutar ataque.
- CU-ARE-03 - Usar defensa.

**Actor:** Jugador.

**Objetivo:** Permitir que el jugador realice acciones
durante su turno y consulte el estado del combate.

#### Prototipo

![P02 - Turno de combate](prototipo/P02_turno_combate.png)

#### Componentes de la interfaz

| Componente | Funcionalidad |
|---|---|
| Número de turno | Identifica el turno actual. |
| Indicador de turno | Informa qué personaje puede actuar. |
| Personaje propio | Muestra el estado del jugador. |
| Personaje rival | Muestra el estado del oponente. |
| Barras de vida | Representan la salud actual de los personajes. |
| Barras de maná | Representan los recursos disponibles. |
| Botón Atacar | Permite ejecutar un ataque básico. |
| Botón Habilidad | Permite utilizar una habilidad. |
| Botón Defender | Permite reducir el daño del siguiente ataque. |
| Registro de combate | Muestra las acciones y sus resultados. |

#### Validaciones

- Solo se permite ejecutar acciones durante el turno propio.
- Las habilidades requieren suficiente maná.
- La interfaz debe informar si un ataque es esquivado.
- Las barras de vida se actualizan después de cada ataque.
- La defensa reduce en un 50 % el daño del siguiente ataque.
- El combate finaliza cuando la vida de un personaje
  llega a cero.

#### Estados de la interfaz

| Estado | Comportamiento |
|---|---|
| Turno del jugador | Acciones habilitadas. |
| Turno del rival | Acciones deshabilitadas. |
| Maná insuficiente | Habilidad deshabilitada. |
| Ataque esquivado | Mensaje informativo en el registro. |
| Combate finalizado | Transición a la pantalla P03. |

La acción Defender corresponde a una funcionalidad opcional
según la priorización MoSCoW del módulo.

#### Navegación

- Si ambos personajes tienen vida → Permanecer en P02.
- Si la vida de un personaje llega a cero → P03.

---

### 5.3. P03 - Resultado del combate

**Caso de uso:** CU-ARE-04 - Consultar resultado.

**Actor:** Jugador.

**Objetivo:** Informar el resultado final del combate
y permitir al jugador iniciar uno nuevo o regresar al menú.

#### Prototipo

![P03 - Resultado del combate](prototipo/P03_resultado.png)

#### Componentes de la interfaz

| Componente | Funcionalidad |
|---|---|
| Indicador de resultado | Muestra victoria o derrota. |
| Personaje ganador | Identifica al vencedor. |
| Personaje derrotado | Identifica al perdedor. |
| Turnos totales | Muestra la cantidad de turnos realizados. |
| Daño infligido | Muestra el daño causado por el jugador. |
| Daño recibido | Muestra el daño recibido por el jugador. |
| Vida restante | Muestra la salud final del personaje. |
| Maná restante | Muestra el maná final disponible. |
| Confirmación de registro | Informa que el resultado fue almacenado. |
| Botón Nuevo combate | Regresa a P01. |
| Botón Volver al menú | Regresa al menú principal. |

#### Validaciones

- El resultado solo se presenta cuando el combate finaliza.
- El sistema identifica al ganador y al perdedor.
- El resultado debe registrarse.
- La confirmación solo se muestra si el registro es exitoso.
- No se entregan recompensas en esta versión.

#### Estados de la interfaz

- Victoria: el personaje del jugador gana el combate.
- Derrota: el personaje del jugador pierde el combate.

#### Navegación

- Nuevo combate → P01.
- Volver al menú → Menú principal.

---

## 6. Diagrama de navegación

El diagrama representa las transiciones entre las tres
pantallas del prototipo y las condiciones que permiten
cambiar de una interfaz a otra.

![Diagrama de navegación](prototipo/navegacion.png)

[Código fuente del diagrama PlantUML](prototipo/navegacion.puml)

### Flujo de navegación

| Origen | Acción o condición | Destino |
|---|---|---|
| Menú principal | Entrar a la arena | P01 |
| P01 | Seleccionar o buscar rival | P01 |
| P01 | Iniciar combate | P02 |
| P01 | Volver al menú | Menú principal |
| P02 | Ambos personajes tienen vida | P02 |
| P02 | Vida de un personaje igual a cero | P03 |
| P03 | Nuevo combate | P01 |
| P03 | Volver al menú | Menú principal |

## 7. Casos de uso asociados

| Interfaz | Caso de uso | Descripción |
|---|---|---|
| P01 | CU-ARE-01 | Emparejar oponente |
| P02 | CU-ARE-02 | Ejecutar ataque |
| P02 | CU-ARE-03 | Usar defensa |
| P03 | CU-ARE-04 | Consultar resultado |

## 8. Trazabilidad

### Artefactos de entrada

- Requisitos funcionales.
- Casos de uso.
- Modelo del dominio.
- Diagrama de actividades.

### Artefactos que alimenta

- Capa de vista.
- Manual de usuario.

[Consultar eslabón de trazabilidad](00_eslabon_trazabilidad.md)

## 9. Criterios de validación del prototipo

- Las tres pantallas mantienen una identidad visual coherente.
- Cada pantalla está asociada a uno o más casos de uso.
- La navegación entre interfaces es clara.
- Las acciones respetan las reglas de negocio.
- El resultado es coherente con el flujo del combate.
- No se incluyen funcionalidades fuera del alcance.

## 10. Conclusión

El prototipo de interfaz permite representar y validar
la interacción del jugador con la Arena de combate antes
de iniciar la construcción del software.

Su elaboración facilita la comunicación entre el cliente,
el analista de sistemas y el desarrollador, manteniendo
la trazabilidad entre los requisitos, el análisis
y la futura implementación.
