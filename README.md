# Predictive Input Buffer — versión Android (táctil)

Puerto del mod `ai.predictivebuffer` (v4.7) a **Android + Windows**, con interfaz táctil
estilo QOLMod / Eclipse. Objetivo: Geometry Dash **2.2081** con Geode.

> El `.geode` original solo traía un `.dll` de Windows (código x86-64). Eso no se puede
> "convertir" a Android: hay que **compilar desde código fuente para ARM**. Como no tenía
> el código original, lo reconstruí leyendo el binario (ver más abajo).

## 1) Compilar (gratis, sin instalar nada)

1. Crea un repositorio en GitHub (New repository).
2. Sube **todos** los archivos de esta carpeta, incluida `.github/workflows/build.yml`.
   - PC: arrastra todo a la página del repo.
   - Celular: *Add file → Upload files* para los archivos de la raíz y luego
     *Add file → Create new file*, escribe `.github/workflows/build.yml` y pega su contenido.
3. Pestaña **Actions** → *Build Geode Mod* → **Run workflow** (también corre solo al subir).
4. Espera ~10-15 min. Cuando salga en verde: abre la ejecución → **Artifacts** →
   `Build Output` → descarga el `.geode` (trae Windows + Android 32 y 64 bits).

## 2) Instalar en Android

Copia el `.geode` a la carpeta de mods del Geode Launcher
(normalmente `Android/media/com.geode.launcher/game/geode/mods/`; en launchers antiguos
`Android/data/com.geode.launcher/files/game/geode/mods/`) y abre GD desde el Geode Launcher.

## 3) Uso (táctil)

| Dónde | Qué hace |
|-------|----------|
| Botón **IB** flotante dentro del nivel | **Tocar** = cambia de etapa `2/2 → 4/4 → 8/8 → 16/8 → OFF` (sustituye a **F6**). **Arrastrar** = moverlo. Parpadea en verde cuando el buffer retrasa un input. |
| Botón **IB** en el menú de pausa / menú principal | Menú completo: elegir etapa, mostrar/ocultar el botón flotante, opacidad, log. |

Los ajustes (etapa, posición del botón, opacidad) se guardan entre sesiones.
En Windows sigue funcionando **F6**.

Etapas (1 tick = 1/240 s): **2/2**, **4/4**, **8/8**, **16/8** (todos los modos / wave) y **OFF**.

## Qué se conservó del original (leído del binario)

- Simulación de la trayectoria con 2 jugadores "fantasma" (cubo/bola/etc. y wave/dual).
- Horizonte de simulación: 48 ticks (28 en mini-wave, 38 en wave).
- Si el input inmediato sobrevive todo el horizonte → no se toca. Si no, se prueban retrasos
  1…N y se aplica el primero que sobrevive claramente más (+12 ticks; en wave +10 con umbrales).
- Wave: se bufferiza press **y** release; robot: también release; resto: solo press.
- Sin buffer en ship, jugador muerto, ni con spam rápido de clics en wave.
- Durante la simulación se anulan efectos secundarios (saltos contados, estela del wave,
  brillos, triggers, animación de fin de nivel) y el fantasma solo choca con sólidos,
  peligros y pendientes.

## Qué cambió

- Tecla F6 → botón táctil (F6 sigue en Windows).
- Consola de Windows → log de Geode (opción *Log saves*).
- Los fantasmas viven en un contenedor invisible (así no se ven sus estelas/partículas).
- Se reinicia el estado al reiniciar el nivel (`resetLevel`).

## Importante / limitaciones

- **No pude compilar con el SDK real de Geode ni probarlo en un teléfono** (mi entorno no
  tiene red ni NDK). Comprobé la sintaxis y la lógica del búfer con un entorno simulado,
  pero la primera compilación real puede dar algún error de firma. Si Actions sale en rojo,
  abre el job que falló y pásame el log: se arregla rápido.
- La copia de estado jugador → fantasma usa `copyAttributes` (si existe) + campos conocidos;
  puede diferir ligeramente del original.
- En móviles lentos la etapa **16/8** hace más simulaciones y puede dar un pequeño tirón:
  usa 2/2 o 4/4.
- Si tienes el código fuente original, mándalo y lo adapto 1:1 en vez de esta reconstrucción.
