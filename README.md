# 🌊 Océano Interactivo: Explicación y Reflexión de la Obra



---


# DEMO DE LA MUESTRA



[https://youtu.be/fb5H2exo0cI](https://youtu.be/eUFg-ST6F0A?si=Y1RLxR8DpbODXkUV)




## 1. Explicación breve de la obra

###  Intención
La obra busca reflexionar sobre la influencia colectiva en la transformación de los ecosistemas naturales, utilizando el océano y sus "estaciones" como metáfora de un entorno vivo y reactivo. La intención artística es trasladar la responsabilidad del estado climático al público, convirtiendo la toma de decisiones individual en una manifestación visual comunitaria donde cada persona influye en la atmósfera del espacio sumergido.

###  Percepción
El público experimenta la obra no como espectador pasivo, sino como co-creador activo del espacio. Al conectarse desde su dispositivo móvil, la barrera entre lo físico y lo digital se disuelve; el asistente siente agencia directa al ver cómo su interacción en el celular altera inmediatamente la proyección colectiva, genera imagenes de partículas y devuelve retroalimentación en tiempo real a su propia pantalla.

###  Sistema técnico
La pieza se sostiene en un ecosistema interactivo bidireccional de baja latencia:
* **Servidor Puente (Node.js):** Utiliza `express`, `socket.io` y `osc` para funcionar como intermediario entre la interfaz web móvil (accesible mediante un código QR) y el motor gráfico.
* **Motor Visual (TouchDesigner):** Recibe los paquetes UDP/OSC para:
  * Conmutar las escenas estacionales (`Switch TOP`).
  * Contabilizar la telemetría de votos en un `Constant CHOP`.
  * Encauzar disparos de partículas mediante lógica de multiplicación (`Fan CHOP` + `Trigger CHOP` + `Math CHOP`).
  * Devolver el marcador actualizado a todos los dispositivos en paralelo a través de un `OSC Out CHOP`.

 ### flujo de performance

<img width="1682" height="808" alt="image" src="https://github.com/user-attachments/assets/0675cd1d-60b2-41d6-a94d-108697f2b7e3" />

---

## 2. Reflexión post-performance y análisis crítico

### Evaluación del resultado: Intención vs. Percepción
El performance demostró un alto nivel de involucramiento y apropiación por parte del público. La intención conceptual de generar una responsabilidad colectiva se tradujo en una dinámica de **juego y negociación social**: los asistentes no solo votaban de manera aislada, sino que se ponian deacuerdo para votar por la que mas votos tuviera en ese moemto. 

En cuanto a la percepción, la inmediatez de la respuesta visual y el marcador en vivo en los teléfonos eliminaron cualquier brecha de aprendizaje; la interacción fue intuitiva e instantánea. Sin embargo, la brecha entre la intención metafórica y la percepción radicó en que el público percibió la experiencia inicialmente como una competencia lúdica más que como una reflexión ecológica profunda, lo cual abre una oportunidad para acentuar el diseño sonoro y la narrativa visual en futuras iteraciones.

### Análisis crítico del sistema técnico

#### Fortalezas del sistema
* **Baja barrera de acceso:** La implementación del código QR en la terminal y la arquitectura web ligera eliminaron la necesidad de instalar aplicaciones externas, garantizando un flujo continuo de participantes.
* **Eficiencia y escalabilidad:** El uso de un único `Trigger CHOP` multiplicado por un `Fan CHOP` para enrutar 4 sistemas de partículas independientes demostró ser una solución matemática elegante, limpia y de bajo consumo computacional.
* **Estabilidad bidireccional:** La retransmisión de datos vía UDP/OSC hacia Node.js y la propagación mediante WebSockets a los teléfonos mantuvo sincronizados todos los marcadores sin caídas de servidor ni desfasajes perceptibles.

# Guía Paso a Paso para Replicar la Obra: Océano Interactivo
Esta guía describe el procedimiento exacto para poner en marcha la experiencia interactiva, desde el paisaje sonoro hasta la conexión del público y la proyección visual.

## Requisitos Previos

Software Instalado:

- Node.js (v14 o superior).

- TouchDesigner (versión reciente compatible con Python 3.x).

- Navegador web moderno (Chrome, Edge o Firefox).

- Open stage control (https://openstagecontrol.ammd.net/)

## Archivos del Proyecto:

Archivo de TouchDesigner: Oceano_Interactivo.toe.

Carpeta del servidor puente Node.js con los archivos server.js e index.html.

## Conexión de Red:

La computadora principal y todos los dispositivos móviles deben estar conectados a la misma red Wi-Fi local.

### Paso 1: Configurar el Paisaje Sonoro en Strudel

Abre el navegador web e ingresa a la plataforma oficial de live coding: https://strudel.cc/.

Copia y pega el siguiente código en el editor central de Strudel. Este bloque genera las secuencias sonoras reactivas para cada una de las 4 estaciones:

```js
//========================================================
// 🎛️ CONTROLADOR PRINCIPAL Y TOUCHDESIGNER
//========================================================

// 1 = Primavera 🌸 | 2 = Verano ☀️ | 3 = Otoño 🍂 | 4 = Invierno ❄️
const estacion = 3; 

// Activamos el parámetro OSC para TouchDesigner
const { visualid } = createParams('visualid');

// Función personalizada para otoño e invierno
const ratchet = register('ratchet', (pat) => pat.sometimes(ply(2)));


//========================================================
// 🌸 1. PRIMAVERA (72/4 BPM)
//========================================================

const prima_base = note("<c4M d4m e4m f4M>")
  .s("triangle").slow(8).room(slider(1, 0.1, 1.5, 0.1)).lpf(slider(1500, 500, 3000, 50)).gain(.30)
  .visualid("harmony");

const prima_kalimba = note("[c5 e5 g5 a5]")
  .s("kalimba").slow(2).delay(0.5).room(0.8).gain(.20)
  .visualid("melody");

const prima_oleaje = note("<c3 g3 e3 g3 d3 a3 f3 a3>")
  .s("sine").slow(8).attack(1).release(2).room(1).gain(.10).pan(sine.range(-.35,.35).slow(8))
  .visualid("drum_bd"); // (Visual repetido)

const prima_melodia2 = note("e5 ~ g5 ~ a5 g5 e5 ~ d5 ~ e5 g5 ~ a5")
  .s("triangle").slow(4).attack(.5).release(1).room(.9).gain(.11)
  .visualid("drum_cp"); // (Visual repetido)

const prima_melodia_calida = note("g5 a5 b5 ~ a5 g5 e5 ~ g5 a5 c6 ~ b5 a5")
  .s("sine").slow(6).attack(.7).release(1.5).room(1).gain(.08)
  .visualid("drum_hh"); // (Visual repetido)

const prima_reflejos = note("[c6 ~ e6] [g6 ~ a6] [e6 ~ g6] [b5 ~ e6]")
  .s("kalimba").slow(4).delay(.7).room(1).gain(.07).pan(sine.range(-.6,.6).slow(8))
  .visualid("drum_oh"); // (Visual repetido)

const prima_brisa = s("~ hh ~ ~ hh ~ [hh hh] ~")
  .gain(.055).lpf(3000).room(1)
  .visualid("drum_hh");

const prima_destellos = note("e6 ~ ~ g6 ~ a6 ~ ~")
  .s("triangle").slow(3).delay(.8).room(1).gain(.05)
  .visualid("melody");


//========================================================
// ☀️ 2. VERANO (132/4 BPM)
//========================================================

const verano_olas = note("<[c4,e4,g4,b4] [d4,f4,a4,c5]>")
  .s("sawtooth").lpf(sine.range(700,2200).slow(4)).gain(.16).room(.7)
  .visualid("harmony");

const verano_melodia = note("e5 g5 a5 b5 a5 g5 e5 d5 e5 g5 a5 c6 b5 a5 g5 e5")
  .s("square").decay(.12).sustain(.15).release(.08).gain(.24).pan(sine.range(-.7,.7).slow(2))
  .visualid("melody");

const verano_bajo = note("e2 e2 g2 a2 ~ a2 g2 e2 d2 d2 f2 a2 g2 e2")
  .s("sawtooth").lpf(700).decay(slider(0.3, 0.05, 0.6, 0.05)).sustain(.35).gain(.38)
  .visualid("drum_bd"); // (Visual repetido para el bajo)

const verano_kick = s("bd*4")
  .gain(slider(0.8, 0.3, 1.5, 0.1))
  .visualid("drum_bd");

const verano_perc = s("~ cp ~ cp ~ cp [cp cp] ~")
  .gain(.55)
  .visualid("drum_cp");

const verano_hh = s("hh*8")
  .gain(.32).pan(sine.range(-1,1).fast(2))
  .visualid("drum_hh");

const verano_shaker = s("[~ sd] [sd ~] [~ sd] [sd sd]")
  .gain(.28)
  .visualid("drum_oh");

const verano_brillos = note("c6 e6 g6 b6")
  .s("triangle").delay(.35).room(.8).gain(.12)
  .visualid("harmony");


//========================================================
// 🍂 3. OTOÑO (75 BPM) - Pistas separadas en el tiempo
//========================================================

const otono_viento = arrange(
  [4, s("~ ~ ~ ~").gain(0.08)],
  [4, s("bd ~ ~ ~").gain(0.10)],
  [4, s("bd ~ ~ ~").gain(0.16)],
  [2, s("bd ~ ~ ~").gain(0.07)],
  [4, s("~ ~ ~ ~").gain(0.06)],
  [4, s("bd ~ ~ ~").gain(0.15)],
  [2, s("~ bd ~ ~").gain(0.06)],
  [4, s("~ ~ ~ ~").gain(0.05)]
).visualid("drum_bd");

const otono_perc = arrange(
  [4, s("~")],
  [4, s("~")],
  [4, s("~ ~ sd ~").gain(0.12)],
  [2, s("~")],
  [4, s("~")],
  [4, s("~ ~ sd ~").gain(0.11)],
  [2, s("~")],
  [4, s("~")]
).visualid("drum_cp");

const otono_bajo = arrange(
  [4, note("<d2 ~ a1 ~>").slow(4).sound("sine").gain(0.16)],
  [4, note("<d2 ~ a1 ~>").slow(3).sound("sine").gain(0.18)],
  [4, note("<d2 ~ a1 ~>").slow(2).sound("sine").gain(0.21)],
  [2, note("<a3 ~ f3 ~>").slow(3).sound("sine").gain(0.09)],
  [4, note("<d2 ~ a1 ~>").slow(4).sound("sine").gain(0.15)],
  [4, note("<d2 ~ a1 ~>").slow(2).sound("sine").gain(0.19)],
  [2, note("<a3 ~ f3 ~>").slow(3).sound("sine").gain(0.07)],
  [4, note("<d2 ~ ~ ~>").slow(4).sound("sine").gain(0.12)]
).visualid("harmony");

const otono_melodia = arrange(
  [4, note("<d4 ~ f4 ~>").slow(4).sound("triangle").gain(0.14)],
  [4, note("<d4 f4 ~ a4>").slow(4).sound("triangle").gain(0.16)],
  [4, note("<d4 ~ f4 ~ a4 ~ c5 ~>").slow(3).sound("triangle").gain(0.20)],
  [2, note("<a4 ~ c5 ~>").slow(3).sound("triangle").gain(0.10).room(0.8)],
  [4, note("<f4 ~ e4 ~ d4 ~>").slow(4).sound("triangle").gain(0.13)],
  [4, note("<d4 ~ f4 ~ a4 ~ f4 ~>").slow(3).sound("triangle").gain(0.18)],
  [2, note("d5 ~ c5 ~ a4 ~").slow(3).sound("triangle").gain(0.08).room(0.8)],
  [4, note("<f4 ~ e4 ~ d4 ~>").slow(4).sound("triangle").gain(0.10)]
).visualid("melody");

const otono_fondo = arrange(
  [4, note("~ a4 ~ ~").slow(4).sound("sine").gain(0.08)],
  [4, note("~ a4 ~ ~").slow(3).sound("sine").gain(0.09)],
  [4, note("<a3 ~ f3 ~>").slow(4).sound("sine").gain(0.12)],
  [2, s("~")],
  [4, note("~ a4 ~ ~").slow(4).sound("sine").gain(0.07)],
  [4, note("<a3 ~ c4 ~>").slow(4).sound("sine").gain(0.10)],
  [2, s("~")],
  [4, note("~ a4 ~ ~").slow(4).sound("sine").gain(0.06)]
).visualid("drum_hh");

const otono_oleaje = arrange(
  [4, note("~ d5 ~ ~").slow(4).sound("sine").delay(0.4).gain(0.04)],
  [4, note("d5 ~ f5 ~").slow(4).sound("sine").gain(0.05).room(0.6)],
  [4, note("a4 ~ c5 ~ d5 ~ c5 ~").slow(3).sound("sine").gain(0.06).delay(0.35)],
  [2, note("d5 ~ a4 ~").slow(3).sound("sine").delay(0.6).gain(0.05)],
  [4, note("~ d5 ~ a4").slow(4).sound("sine").delay(0.5).gain(0.04)],
  [4, note("a4 ~ c5 ~ d5 ~").slow(3).sound("sine").gain(0.06).delay(0.35)],
  [2, note("f5 ~ d5 ~").slow(4).sound("sine").delay(0.7).gain(0.04)],
  [4, note("d5 ~ ~ ~").slow(4).sound("sine").delay(0.8).gain(0.035)]
).visualid("drum_oh");


//========================================================
// ❄️ 4. INVIERNO (65 BPM) - Pistas separadas en el tiempo
//========================================================

const invierno_pulso = arrange(
  [4, s("~ ~ ~ ~").gain(0.08)],
  [4, s("~ ~ bd ~").gain(0.12)],
  [4, s("~ bd ~ ~").gain(0.10)],
  [6, s("~ ~ ~ bd").gain(0.07)]
).visualid("drum_bd");

const invierno_bajo = arrange(
  [4, note("<c3 ~ g2 ~>").slow(4).sound("sine").gain(0.18)],
  [4, note("<c2 ~ g1 ~>").slow(2).sound("sine").gain(0.22)],
  [4, note("<c2 ~ g1 ~ a1 ~ e2 ~>").slow(2).sound("sine").gain(0.24)],
  [6, note("<c2 ~ g1 ~>").slow(4).sound("sine").gain(0.16)]
).visualid("harmony");

const invierno_acordes = arrange(
  [4, note("<e4 ~ d4 ~>").slow(4).sound("triangle").gain(0.15)],
  [4, note("<c4 e4 g4 ~>").slow(4).sound("triangle").gain(0.16)],
  [4, note("<e4 g4 a4 g4 e4 d4 c4 ~>").slow(2).sound("triangle").gain(0.19)],
  [6, note("<e4 ~ d4 ~ c4 ~ g3 ~>").slow(4).sound("triangle").gain(0.13)]
).visualid("melody");

const invierno_copos = arrange(
  [4, s("~")],
  [4, note("e5 ~ ~ d5 ~ ~ g5 ~").slow(2).sound("sine").gain(0.12)],
  [4, note("~ ~ c5 ~ ~ e5 ~ ~").slow(2).sound("sine").gain(0.10)],
  [6, s("~")]
).visualid("drum_oh");


//========================================================
// 🎧 ENRUTAMIENTO FINAL AUDIO + OSC → TOUCHDESIGNER
//========================================================

let td_out;

if (estacion === 1) {
  setcpm(72/4);
  td_out = stack(
    prima_base, prima_base.osc(),
    prima_kalimba, prima_kalimba.osc(),
    prima_oleaje, prima_oleaje.osc(),
    prima_melodia2, prima_melodia2.osc(),
    prima_melodia_calida, prima_melodia_calida.osc(),
    prima_reflejos, prima_reflejos.osc(),
    prima_brisa, prima_brisa.osc(),
    prima_destellos, prima_destellos.osc()
  );

} else if (estacion === 2) {
  setcpm(132/4);
  td_out = stack(
    verano_olas, verano_olas.osc(),
    verano_melodia, verano_melodia.osc(),
    verano_bajo, verano_bajo.osc(),
    verano_kick, verano_kick.osc(),
    verano_perc, verano_perc.osc(),
    verano_hh, verano_hh.osc(),
    verano_shaker, verano_shaker.osc(),
    verano_brillos, verano_brillos.osc()
  );

} else if (estacion === 3) {
  setcpm(75);
  td_out = stack(
    otono_viento, otono_viento.osc(),
    otono_perc, otono_perc.osc(),
    otono_bajo, otono_bajo.osc(),
    otono_melodia, otono_melodia.osc(),
    otono_fondo, otono_fondo.osc(),
    otono_oleaje, otono_oleaje.osc()
  ).room(slider(0.9, 0.1, 1.5, 0.1))
   .lpf(slider(3350, 500, 4000, 50));

} else if (estacion === 4) {
  setcpm(65);
  td_out = stack(
    invierno_pulso, invierno_pulso.osc(),
    invierno_bajo, invierno_bajo.osc(),
    invierno_acordes, invierno_acordes.osc(),
    invierno_copos, invierno_copos.osc()
  ).room(slider(0.9, 0.1, 1.5, 0.1))
   .gain(slider(1.5, 0.2, 1.5, 0.1));
}

// 🚀 ENVIAMOS LA MÚSICA AL MUNDO Y A TOUCHDESIGNER
$: td_out
```

### Paso 2: Encender el Servidor Puente (Node.js)

Abre la terminal o consola de comandos de tu sistema operativo.

Navega hasta la carpeta del proyecto puente-oceano (descargado previamente de este repositorio):

```
cd ruta/a/tu/carpeta/puente-oceano
```
Si es la primera vez que ejecutas el proyecto, instala las dependencias necesarias:

```
npm install
```

Inicia el servidor ejecutando:

```
node server.js
```
<img width="743" height="687" alt="image" src="https://github.com/user-attachments/assets/e0f02383-4332-4984-96d3-2f0dc861aaaa" />

En la terminal verás la dirección IP local de tu equipo y un código QR generado automáticamente en pantalla. Deja esta ventana de la terminal abierta durante toda la presentación.

### Paso 3: Cargar y Configurar TouchDesigner
Descarga y abre el archivo estacionesOceanicas.23.toe en TouchDesigner.

Verifica los nodos principales del flujo:

- oscin3 (OSC In DAT): Debe estar escuchando en el puerto 10000.

  <img width="846" height="522" alt="image" src="https://github.com/user-attachments/assets/01dc9e2f-9c8c-4847-a2be-fb9b14a07a9c" />


- votos (Constant CHOP): Contiene los 4 canales (value0, value1, value2, value3) correspondientes a Primavera, Verano, Otoño e Invierno.

  <img width="962" height="551" alt="image" src="https://github.com/user-attachments/assets/46283ae9-bdce-40e3-af6b-40de635c2fd3" />


- trigger1 (Trigger CHOP) y Fan CHOP: Encargados del enrutamiento de las partículas hacia la estación seleccionada mediante el Math CHOP (Multiplicación).

  <img width="1145" height="630" alt="image" src="https://github.com/user-attachments/assets/1976300e-d0ad-45c5-9f26-8c997cb6d04d" />


- OSC Out CHOP: Configurado hacia la IP 127.0.0.1 en el puerto 57121 para retornar la telemetría de votos al servidor Node.js.

  <img width="1092" height="541" alt="image" src="https://github.com/user-attachments/assets/f1e3cc2f-632b-4638-b591-c64eacfa2461" />


- Asegúrate de que la ventana de renderizado visual esté activa y lista para proyectar.

- descarga la carpeta ```Image``` que contiene las imagenes para las particulas de interacción y conectalas a los nodos de cada uno 

### Paso 4: Cargar el Open Stage Control

Ejecuta la aplicacion de OpenStageControl.

Verifica que la conexion sea la siguiente:

- En el apartado de send colocas: ``` 127.0.0.1:9001 ```
- En el apartado de Port colocas: ```9002```
- En el apartado de osc-port: ```9003```

  <img width="830" height="430" alt="image" src="https://github.com/user-attachments/assets/c64961a1-c631-427a-83d6-76629bc3d86f" />


Despues de verificar todas las conexiones y los puertos Inicia el open stage control

En la parte superior izquierda haz click en los 3 puntitos y en el apartado de session seleccionas open y buscar el archivo llamado ```clientUI.json``` (Está en el repositorio para descargar)

<img width="973" height="590" alt="image" src="https://github.com/user-attachments/assets/1cfe8812-0071-4814-bbe0-5f01448d1f54" />


Ya con esto puedes utilizar la seleccion de 0 a 3 para cambiar la estación:
0-Primavera
1-Verano
2-Otoño
3-Invierno

### Paso 5: Conectar al Público Móvil

Pide a los asistentes que abran la cámara de sus celulares (conectados a la misma red Wi-Fi local) y escaneen el código QR que se muestra en la terminal del servidor Node.js.

Al escanearlo, el celular abrirá automáticamente la interfaz web interactiva (index.html).

El público podrá:

Seleccionar cualquiera de las 4 estaciones climáticas.

Presionar el botón "✨ Generar Interacción ✨" para detonar explosiones de partículas en tiempo real sobre la pantalla del océano.

Consultar en su propio celular el Marcador en Vivo actualizado automáticamente.

<img width="601" height="822" alt="image" src="https://github.com/user-attachments/assets/4553e48a-78b3-493f-abe7-4868415e991b" />


### Paso 6: Reinicio de la Experiencia

Si deseas reiniciar la votación a cero en cualquier momento de la presentación o para un nuevo grupo de espectadores:

Ve a la ventana de TouchDesigner.

Presiona la tecla r en tu teclado.

El nodo CHOP Execute reestablecerá instantáneamente todos los canales del nodo votos a 0, y la pantalla de todos los teléfonos conectados se actualizará a cero en tiempo real.



