# Laboratorio No. 03 — Robótica Industrial 2026-I
## Análisis y Operación del Manipulador EPSON T3-401S

**Asignatura:** Robótica Industrial  
**Semestre:** 2026-I  
**Integrantes:** lmendozar2001  
**Repositorio:** https://github.com/lmendozar2001/RoboticaLab3

---

## 1. Cuadro Comparativo de Manipuladores

A continuación se comparan las especificaciones técnicas del **Motoman MH6**, el **ABB IRB140** y el **EPSON T3-401S**.

| Característica | Motoman MH6 | ABB IRB140 | EPSON T3-401S |
|---|---|---|---|
| **Tipo** | Articulado (6 DOF) | Articulado (6 DOF) | SCARA (4 DOF) |
| **Carga máxima** | 6 kg | 6 kg | 3 kg |
| **Alcance máximo** | 1 422 mm | 810 mm | 400 mm |
| **Grados de libertad** | 6 | 6 | 4 |
| **Velocidad máx. TCP** | 2 000 mm/s | 2 500 mm/s | 5 100 mm/s |
| **Repetibilidad** | ±0.08 mm | ±0.01 mm | ±0.01 mm |
| **Peso del robot** | 130 kg | 98 kg | 12 kg |
| **Controlador** | DX100 / YRC1000 | IRC5 | RC700 / RC90 |
| **Software** | MotoSim / MotoPlus | RobotStudio | EPSON RC+ 7.0 |
| **Aplicaciones típicas** | Soldadura, manipulación, ensamble | Ensamble, pick & place, soldadura | Pick & place, ensamble de precisión, paletizado |
| **Comunicación** | EtherNet/IP, DeviceNet | EtherNet/IP, PROFIBUS | USB, Ethernet, E/S digitales |
| **Entorno** | Industrial pesado | Industrial general | Industria ligera / laboratorio |

**Análisis:** El EPSON T3-401S es un robot SCARA compacto y de alta velocidad, ideal para tareas de pick & place en espacios reducidos. El ABB IRB140 y el Motoman MH6 son robots articulados de 6 DOF con mayor versatilidad de orientación, adecuados para soldadura y ensamble complejo. La principal ventaja del EPSON T3-401S en este laboratorio es su velocidad y precisión para operaciones de paletizado en plano horizontal.

---

## 2. Configuración Home del EPSON T3-401S

La posición **Home** del EPSON T3-401S es la posición de referencia desde la cual el robot inicia y finaliza sus rutinas. Se define mediante el software EPSON RC+ 7.0 y corresponde a una posición segura y reproducible.

**Posición de cada articulación en Home:**

| Articulación | Descripción | Ángulo / Posición en Home |
|---|---|---|
| **J1** | Rotación de la base (eje Z vertical) | 0° |
| **J2** | Rotación del brazo (eje Z vertical) | 0° |
| **J3** | Desplazamiento vertical del eje Z (traslación lineal) | Posición más alta (mínima extensión) |
| **J4** | Rotación de la muñeca (eje Z) | 0° |

La posición Home se puede definir de dos maneras en el EPSON T3-401S:
- **Home mecánico:** posición física marcada en las articulaciones del robot, usada para calibración.
- **Home de usuario:** posición definida por el programador en el software RC+ 7.0 mediante el comando `Home`, que el robot ejecuta al inicio y fin de cada programa.

En el código del laboratorio, la instrucción `Home` se llama al inicio del programa principal y al finalizar la rutina, garantizando que el robot regrese a una posición segura.

---

## 3. Movimientos Manuales del EPSON T3-401S

El movimiento manual del robot se realiza desde el **Teach Pendant** o desde la interfaz del software EPSON RC+ 7.0 en modo de jog.

### 3.1 Cambio entre modos de movimiento

| Tecla / Acción | Función |
|---|---|
| **F1 / Joint** | Activa el modo de movimiento por articulaciones (Joint Jog) |
| **F2 / World** | Activa el modo de movimiento cartesiano (World Jog) |
| **F3 / Tool** | Activa el modo de movimiento relativo a la herramienta (Tool Jog) |
| **F4 / Local** | Activa el modo de movimiento relativo al sistema de coordenadas local |

### 3.2 Movimientos por articulaciones (Joint Jog)
En este modo cada tecla controla una articulación individual:
- **J1+/J1−**: Rotación de la base.
- **J2+/J2−**: Rotación del segundo eslabón.
- **J3+/J3−**: Desplazamiento vertical del eje Z.
- **J4+/J4−**: Rotación de la muñeca.

### 3.3 Movimientos cartesianos (World Jog)
En este modo el TCP (Tool Center Point) se desplaza a lo largo de los ejes del sistema de coordenadas global:
- **X+/X−**: Traslación en el eje X.
- **Y+/Y−**: Traslación en el eje Y.
- **Z+/Z−**: Traslación en el eje Z (vertical).
- **Rx+/Rx−, Ry+/Ry−, Rz+/Rz−**: Rotaciones alrededor de los ejes (en robots con 6 DOF; en el SCARA solo aplica Rz).

---

## 4. Niveles de Velocidad para Movimientos Manuales

El EPSON T3-401S ofrece múltiples niveles de velocidad para el jog manual, lo que permite al operador moverse con precisión cerca de objetos o con mayor rapidez en espacios libres.

### 4.1 Niveles disponibles

| Nivel | Velocidad aproximada | Uso recomendado |
|---|---|---|
| **Low (Bajo)** | ~1% de la velocidad máxima | Posicionamiento fino, cercanía a objetos |
| **Medium (Medio)** | ~10–30% | Movimientos de ajuste general |
| **High (Alto)** | ~50–100% | Desplazamientos rápidos en espacio libre |

### 4.2 Cambio entre niveles de velocidad
- En el **Teach Pendant**: se utilizan las teclas **Speed+** y **Speed−** para incrementar o reducir el nivel de velocidad.
- En **EPSON RC+ 7.0** (modo manual): se selecciona el nivel desde el panel de control de jog, mediante un slider o botones de velocidad en la interfaz gráfica.

### 4.3 Identificación en pantalla
El nivel de velocidad activo se muestra en la barra de estado del Teach Pendant o en la interfaz de RC+ 7.0 como un porcentaje numérico o como indicador gráfico (barra de progreso). El valor se actualiza en tiempo real al presionar las teclas de cambio de velocidad.

---

## 5. Software EPSON RC+ 7.0

### 5.1 Principales funcionalidades
EPSON RC+ 7.0 es el entorno de desarrollo y control oficial para los robots EPSON. Sus funcionalidades principales son:

- **Editor de programas SPEL+**: lenguaje de programación propietario de EPSON, orientado a la robótica, con soporte para variables, funciones, control de E/S digitales, comandos de movimiento y manejo de pallets.
- **Simulador 3D integrado**: permite simular trayectorias y detectar colisiones antes de ejecutarlas en el robot real.
- **Panel de control de jog**: interfaz gráfica para mover el robot manualmente en cualquier modo (Joint, World, Tool).
- **Monitor de E/S**: visualización y control en tiempo real de las entradas y salidas digitales del robot.
- **Gestor de puntos (Points)**: herramienta para registrar, editar y organizar los puntos de trayectoria del robot.
- **Herramienta Pallet**: permite definir matrices de posiciones (pallets) con solo tres puntos de referencia, facilitando operaciones de paletizado.
- **Diagnóstico y log de errores**: registro de eventos y errores del sistema para depuración.

### 5.2 Comunicación con el manipulador
EPSON RC+ 7.0 se comunica con el controlador del robot (RC700 o RC90) mediante:
- **USB**: conexión directa para programación y control desde una PC.
- **Ethernet (TCP/IP)**: permite control remoto y monitoreo en red.

El proceso de comunicación funciona así:
1. El usuario escribe el programa en SPEL+ dentro de RC+ 7.0.
2. RC+ 7.0 compila el código y lo transfiere al controlador del robot.
3. El controlador interpreta las instrucciones y genera las señales de control para los servomotores de cada articulación.
4. Los encoders de cada articulación envían retroalimentación de posición al controlador, cerrando el lazo de control.
5. RC+ 7.0 recibe el estado del robot en tiempo real y lo muestra en la interfaz.

---

## 6. Comparación: EPSON RC+ 7.0 vs RoboDK vs RobotStudio

| Característica | EPSON RC+ 7.0 | RoboDK | RobotStudio |
|---|---|---|---|
| **Fabricante** | EPSON | RoboDK Inc. | ABB |
| **Compatibilidad** | Solo robots EPSON | Multi-marca (ABB, KUKA, FANUC, EPSON, etc.) | Solo robots ABB |
| **Lenguaje de programación** | SPEL+ (propietario) | Python, scripts propios, post-procesadores | RAPID (propietario ABB) |
| **Simulación 3D** | Sí (integrada) | Sí (avanzada, con importación CAD) | Sí (muy avanzada) |
| **Detección de colisiones** | Básica | Sí | Sí (avanzada) |
| **Generación de código** | Directo al controlador EPSON | Post-procesadores para múltiples marcas | Directo al controlador ABB |
| **Costo** | Gratuito (con robot EPSON) | De pago (licencia) | Gratuito (con robot ABB) |
| **Curva de aprendizaje** | Media | Baja-Media | Media-Alta |
| **Uso principal** | Programación y control de robots EPSON | Simulación y programación offline multi-marca | Programación y simulación de robots ABB |

**Análisis personal:**
- **EPSON RC+ 7.0** es la herramienta nativa para el T3-401S. Ofrece control total del hardware y es indispensable para la programación real del robot. Su lenguaje SPEL+ es intuitivo para tareas de pick & place y paletizado.
- **RoboDK** es la herramienta más versátil para entornos multi-marca. Permite simular y generar código para casi cualquier robot industrial, lo que la hace ideal para comparar soluciones o trabajar en entornos con múltiples marcas.
- **RobotStudio** es el equivalente de RC+ 7.0 pero para el ecosistema ABB. Ofrece simulación muy fiel al comportamiento real del robot y es ampliamente usado en la industria para programación offline de robots ABB.

---

## 7. Diseño del Gripper Neumático por Vacío

### 7.1 Descripción general
Se diseñó un gripper neumático por vacío (ventosa) para manipular huevos de manera segura. El sistema utiliza una ventosa de silicona de copa blanda que se adapta a la superficie curva del huevo, generando vacío mediante un eyector Venturi o una bomba de vacío.

### 7.2 Componentes utilizados

| Componente | Especificación | Función |
|---|---|---|
| **Ventosa de silicona** | Ø 30–40 mm, copa blanda | Contacto y agarre del huevo |
| **Eyector Venturi** | Presión de trabajo: 4–6 bar | Generación de vacío |
| **Válvula solenoide 5/2** | 24 VDC, conexión 1/8" | Control ON/OFF del vacío |
| **Sensor de vacío** | Rango: 0 a −100 kPa | Verificación de agarre exitoso |
| **Tubería neumática** | Ø 4 mm, poliuretano | Conducción de aire/vacío |
| **Racores y adaptadores** | 1/8" NPT | Conexiones neumáticas |
| **Soporte de montaje** | Aluminio mecanizado | Fijación al flange del robot |

### 7.3 Configuración de E/S digitales del EPSON T3-401S

| Señal | Pin | Función |
|---|---|---|
| **D0_09 (Output 9)** | Salida digital 9 | Activar válvula solenoide (ON = vacío activo = agarre) |
| **D0_10 (Output 10)** | Salida digital 10 | Purga / soplado para liberar el huevo (opcional) |
| **DI_01 (Input 1)** | Entrada digital 1 | Señal del sensor de vacío (confirmación de agarre) |

En el código, el control del gripper se realiza con:
```spel
On D0_09    ' Activa el vacío → agarra el huevo
Off D0_09   ' Desactiva el vacío → suelta el huevo
```

### 7.4 Diagrama esquemático (descripción)
```
[Compresor/Aire] → [Válvula solenoide 5/2 (D0_09)] → [Eyector Venturi] → [Ventosa Ø35mm]
                                                                    ↓
                                                         [Sensor de vacío (DI_01)]
```
El flange del robot sostiene el soporte de aluminio al que se fija el eyector y la ventosa. La válvula solenoide recibe la señal digital D0_09 del controlador RC700.

---

## 8. Trayectoria con Patrón de Caballo de Ajedrez

### 8.1 Descripción del problema
Se tienen dos huevos ubicados en los extremos de una cubeta de 30 posiciones (6 columnas × 5 filas):
- **Huevo A**: posición inicial 1 (esquina superior izquierda).
- **Huevo B**: posición inicial 30 (esquina inferior derecha).

La rutina debe mover los huevos alternadamente (primero A, luego B, luego A, etc.) a través de todas las posiciones de la cubeta, siguiendo el **patrón de movimiento del caballo en ajedrez** (desplazamiento de 2 casillas en una dirección y 1 en la perpendicular, o viceversa).

### 8.2 Plano de planta — Cubeta de huevos (6×5)

```
     Col1  Col2  Col3  Col4  Col5  Col6
Fil1 [ 1]  [ 2]  [ 3]  [ 4]  [ 5]  [ 6]
Fil2 [ 7]  [ 8]  [ 9]  [10]  [11]  [12]
Fil3 [13]  [14]  [15]  [16]  [17]  [18]
Fil4 [19]  [20]  [21]  [22]  [23]  [24]
Fil5 [25]  [26]  [27]  [28]  [29]  [30]
```

- **Huevo A** inicia en posición **1** (Fil1, Col1) — marcado con `[A]`
- **Huevo B** inicia en posición **30** (Fil5, Col6) — marcado con `[B]`

La secuencia de recorrido definida en el código (`tour[]`) es:
`1 → 30 → 9 → 17 → 5 → 6 → 18 → 10 → 29 → 2 → 21 → 13 → 25 → 26 → 14 → 15 → 3 → 19 → 7 → 27 → 20 → 23 → 28 → 12 → 24 → 4 → 11 → 8 → 22 → 16`

Cada paso impar lo ejecuta el Huevo A y cada paso par lo ejecuta el Huevo B.

### 8.3 Diagrama de flujo de la rutina

```
INICIO
  │
  ▼
Motor ON, Power High, Accel 50/50, Speed 30
  │
  ▼
Definir Pallet 1 (Origin, PuntoX, PuntoY, 6 cols × 5 filas)
  │
  ▼
InitTour() → Cargar secuencia de 30 posiciones (patrón caballo)
  │
  ▼
InitOcc() → HuevoA en pos 1, HuevoB en pos 30, marcar ocupadas
  │
  ▼
┌─────────────────────────────────────────────┐
│  Para sIndex = 1 hasta 30                   │
│    ¿sIndex es impar?                        │
│    ├─ SÍ → Mover Huevo A a tour(sIndex)     │
│    └─ NO → Mover Huevo B a tour(sIndex)     │
│                                             │
│    MoveEgg(nombre, destino):                │
│      Jump → posición actual del huevo       │
│      PickAndPlace → ir a destino            │
│        Off D0_09 (soltar)                   │
│        Jump Pallet(1, destIdx)              │
│        On D0_09 (agarrar)                   │
│      UpdateOcc → actualizar posición        │
└─────────────────────────────────────────────┘
  │
  ▼
Home
  │
  ▼
Motor OFF
  │
  ▼
FIN
```

### 8.4 Verificación del patrón de caballo
La función `IsReachable()` verifica que el movimiento entre dos posiciones cumpla el patrón del caballo:
- Diferencia de filas (`dr`) y columnas (`dc`) debe satisfacer: `(dr=1 AND dc=2) OR (dr=2 AND dc=1)`.

La función `RowFromIndex` y `ColFromIndex` convierten el índice lineal (1–30) a coordenadas de fila y columna dentro de la grilla 6×5.

---

## 9. Descripción de Trayectorias en EPSON RC+ 7.0

EPSON RC+ 7.0 ofrece los siguientes tipos de trayectorias:

| Tipo | Comando | Descripción |
|---|---|---|
| **Joint (PTP)** | `Go` | Movimiento punto a punto por interpolación de articulaciones. El TCP no sigue una trayectoria rectilínea. Más rápido. |
| **Lineal** | `Move` | El TCP se desplaza en línea recta entre dos puntos. Útil para operaciones de inserción o soldadura. |
| **Arco** | `Arc` / `Arc3` | El TCP sigue una trayectoria circular o de arco definida por puntos intermedios. |
| **Jump** | `Jump` | Movimiento en forma de arco vertical (levanta el TCP, se desplaza, baja). Ideal para pick & place, evita colisiones con objetos intermedios. |
| **Pallet** | `Pallet` + `Jump/Go` | Movimiento a posiciones dentro de una matriz definida. Simplifica la programación de paletizado. |

En este laboratorio se usa principalmente `Jump` para los movimientos de pick & place, ya que levanta el gripper sobre los huevos antes de desplazarse, evitando colisiones con los huevos adyacentes en la cubeta.

**Comparación con RoboDK y RobotStudio:**
- RoboDK y RobotStudio también ofrecen movimientos Joint (MoveJ) y Lineales (MoveL), pero con mayor control sobre la velocidad y aceleración en cada segmento.
- EPSON RC+ 7.0 simplifica el paletizado con la instrucción `Pallet`, mientras que en RoboDK o RobotStudio esto requiere programación manual de las posiciones o uso de plugins adicionales.

---

## Anexo A — Código SPEL+ Completo

```spel
Global Integer i
Global Integer sIndex
Global Integer tour(31)
Global Integer occ(31)
Global Integer eggApos
Global Integer eggBpos
Global Integer boardCols
Global Integer boardRows

' Carga la secuencia de recorrido (patrón de caballo) para las 30 posiciones
Function InitTour
    tour(1) = 1
    tour(2) = 30
    tour(3) = 9
    tour(4) = 17
    tour(5) = 5
    tour(6) = 6
    tour(7) = 18
    tour(8) = 10
    tour(9) = 29
    tour(10) = 2
    tour(11) = 21
    tour(12) = 13
    tour(13) = 25
    tour(14) = 26
    tour(15) = 14
    tour(16) = 15
    tour(17) = 3
    tour(18) = 19
    tour(19) = 7
    tour(20) = 27
    tour(21) = 20
    tour(22) = 23
    tour(23) = 28
    tour(24) = 12
    tour(25) = 24
    tour(26) = 4
    tour(27) = 11
    tour(28) = 8
    tour(29) = 22
    tour(30) = 16
Fend

' Convierte índice lineal a fila (base 0)
Function RowFromIndex(idx As Integer) As Integer
   RowFromIndex = ((idx - 1) / boardCols)
Fend

' Convierte índice lineal a columna (base 1)
Function ColFromIndex(idx As Integer) As Integer
    ColFromIndex = ((idx - 1) Mod boardCols) + 1
Fend

' Verifica que las coordenadas estén dentro de la grilla
Function InBounds(r As Integer, c As Integer) As Boolean
    If r < 1 Then InBounds = 0 EndIf
    If r > boardRows Then InBounds = 0 EndIf
    If c < 1 Then InBounds = 0 EndIf
    If c > boardCols Then InBounds = 0 EndIf
    InBounds = 1
Fend

' Verifica si el movimiento entre la posición actual del huevo e idx es válido (patrón caballo)
Function IsReachable(name$ As String, idx As Integer) As Integer
    Integer r1, c1, r2, c2, dr, dc
    If name$ = "A" Then
        r1 = RowFromIndex(eggApos) : C1 = ColFromIndex(eggApos)
    Else
        r1 = RowFromIndex(eggBpos) : C1 = ColFromIndex(eggBpos)
    EndIf
    r2 = RowFromIndex(idx) : C2 = ColFromIndex(idx)
    dr = Abs(r1 - r2) : dc = Abs(c1 - c2)
    If (dr = 1 And dc = 2) Or (dr = 2 And dc = 1) Then
        IsReachable = 1
    EndIf
    IsReachable = 0
Fend

' Actualiza el arreglo de ocupación tras mover un huevo
Function UpdateOcc(huevo$ As String, newIdx As Integer)
    If huevo$ = "A" Then
        occ(eggApos) = 0 : eggApos = newIdx : occ(eggApos) = 1
    Else
        occ(eggBpos) = 0 : eggBpos = newIdx : occ(eggBpos) = 1
    EndIf
Fend

' Manejo de error: apaga motores y va a Home
Function HandleError
    Motor Off : Home
Fend

' Ejecuta el pick & place: suelta, va al destino, agarra
Function PickAndPlace(gripper$ As String, destIdx As Integer)
    Off D0_09           ' Desactiva vacío (suelta)
    Wait 0.2
    Jump Pallet(1, destIdx)
    Wait 0.2
    On D0_09            ' Activa vacío (agarra)
    Wait 0.2
Fend

' Mueve un huevo desde su posición actual hasta targetIdx
Function MoveEgg(name$ As String, targetIdx As Short)
    If name$ = "A" Then
        Jump Pallet(1, eggApos)
        Wait 0.2
        PickAndPlace("A", targetIdx)
        UpdateOcc("A", targetIdx)
    Else
        Jump Pallet(1, eggBpos)
        Wait 0.2
        PickAndPlace("B", targetIdx)
        UpdateOcc("B", targetIdx)
    EndIf
Fend

' Inicializa posiciones y arreglo de ocupación
Function InitOcc
    eggApos = 1 : eggBpos = 30
    For i = 1 To 30
        occ(i) = 0
    Next
    occ(eggApos) = 1 : occ(eggBpos) = 1
Fend

' Programa principal
Function main
    boardCols = 6 : boardRows = 5
    Motor On : Power High : Accel 50, 50 : Speed 30
    Home
    Pallet 1, Origin, PuntoX, PuntoY, 6, 5
    Call InitTour
    Call InitOcc
    For sIndex = 1 To 30
        If (sIndex Mod 2) = 1 Then
            If eggApos <> tour(sIndex) Then Call MoveEgg("A", tour(sIndex)) EndIf
        Else
            If eggBpos <> tour(sIndex) Then Call MoveEgg("B", tour(sIndex)) EndIf
        EndIf
    Next
    Home
    Motor Off
Fend
```

---

## Anexo B — Punto de Paletizado

El punto de paletizado define la esquina de origen de la cubeta y los dos puntos de referencia para los ejes X e Y del pallet. En el código se usan los puntos enseñados:

- **Origin**: esquina superior izquierda de la cubeta (posición 1).
- **PuntoX**: punto sobre el eje X del pallet (misma fila que Origin, columna 6 → posición 6).
- **PuntoY**: punto sobre el eje Y del pallet (misma columna que Origin, fila 5 → posición 25).

La instrucción `Pallet 1, Origin, PuntoX, PuntoY, 6, 5` define una matriz de 6×5 = 30 posiciones equidistantes. El robot calcula automáticamente las coordenadas de cada celda mediante interpolación bilineal.

*(Ver imagen adjunta: `Punto de Paletizado.jpeg`)*

---

## Anexo C — Videos de Evidencia

- **`simulación.mp4`**: Video de la simulación completa en EPSON RC+ 7.0 mostrando la trayectoria de los dos huevos con patrón de caballo de ajedrez.
- **`Video robot real.mp4`**: Video de la implementación física en el manipulador EPSON T3-401S, evidenciando el funcionamiento del gripper neumático y la ejecución de la rutina.

Ambos videos inician con la introducción oficial del laboratorio LabSIR (Intro LabSIR).

---

*Informe generado para el Laboratorio No. 03 — Robótica Industrial 2026-I*
