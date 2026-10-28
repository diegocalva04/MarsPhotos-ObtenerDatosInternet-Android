# Registro de validación

Fecha: 28 de octubre de 2026, zona America/Guayaquil.

## Alcance autorizado para esta sesión

El usuario indicó: «Por ahora no hagas pruebas graficas, solo el desarrollo, despues hacemos eso». Por ello se realizaron compilaciones y análisis estático sin iniciar la app, sin emulador y sin inspección visual. No se ejecutaron las pruebas unitarias ni instrumentadas existentes.

## Comprobaciones ejecutadas

| Comprobación | Resultado observado |
| --- | --- |
| Inspección de carpeta y Git | Existía una plantilla Android con módulo `app`, Gradle y cuatro archivos Kotlin; no existían `.git` ni Mars Photos. La plantilla se conservó en el primer commit. |
| Lectura del codelab y referencias | Once apartados revisados y dos ramas oficiales obtenidas con `git fetch`. SHA de referencia en `CODELAB.md`. |
| `gradlew.bat --version` | Gradle 9.5.0; criterio de JVM del proceso Gradle: Java 25. |
| Primera compilación del starter adaptado | Falló por iconos adaptativos sin calificador de API 26 al adoptar `minSdk = 24`. Se movieron ambos XML de icono a `mipmap-anydpi-v26`; la siguiente compilación terminó correctamente. |
| Compilación con Retrofit y conversor escalar | `:app:assembleDebug`: `BUILD SUCCESSFUL`. |
| Compilación de solicitud String y permiso | `:app:assembleDebug`: `BUILD SUCCESSFUL`. No se ejecutó la app para observar el JSON. |
| Compilación con estados y pantallas | `:app:assembleDebug`: `BUILD SUCCESSFUL`. No se abrieron las pantallas. |
| Resolución de dependencias JSON | `:app:dependencies --configuration debugCompileClasspath`: `BUILD SUCCESSFUL`. |
| Versión JSON efectiva | `:app:dependencyInsight --dependency kotlinx-serialization-json --configuration debugRuntimeClasspath`: declara 1.5.1 y selecciona 1.7.3 por restricciones transitivas y resolución de conflictos. |
| Compilación y análisis estático finales | `:app:assembleDebug :app:lintDebug`: `BUILD SUCCESSFUL`, también después de incorporar la captura HTTP de la solución oficial. |
| Lint final | 0 errores, 25 advertencias: etiqueta redundante, versiones nuevas disponibles, recursos sin uso conservados y sugerencias de catálogo de versiones. No se suprimieron comprobaciones. |
| Compilador Kotlin | Advertencia de obsolescencia de `window.statusBarColor` en el tema original de `starter`; no impide compilar. |
| Comparación de código inicial | `MainActivity`, `MarsPhotosApp`, tema completo, `loading_img` e `ic_connection_error` coinciden con `starter`. |
| Flujo de datos final | Revisión del código: GET photos → `List<MarsPhoto>` → `listResult.size` → `MarsUiState.Success` → `Text`. No hay valores fijos ni conversor escalar en el resultado final. |

El APK de desarrollo se generó localmente en `app/build/outputs/apk/debug/app-debug.apk`. Las carpetas de resultados e informes permanecen excluidas de Git.

## Comparación final y diferencias justificadas

- **Gradle, SDK, Kotlin y Compose:** se conservan las versiones del proyecto local. `repo-starter` usa un conjunto más antiguo; no se rebajó toda la configuración.
- **Complemento de serialización:** 2.2.10 coincide con Kotlin configurado; no se aplica el complemento 1.8.10 de la página a un proyecto Kotlin 2.2.10.
- **JSON:** se declara 1.5.1 como en el tutorial, pero AndroidX introduce restricciones que elevan la versión resuelta a 1.7.3. La compilación validada utiliza esta última.
- **Modelo:** `MarsPhoto` se ubica en `network`, como pide la página y la solicitud; la rama `repo-starter` lo coloca en `model`. Campos y anotaciones coinciden.
- **Servicio:** mismo servidor y endpoint; URL base con barra final y formato de código normalizado. Sin cambio de comportamiento de la solicitud.
- **ViewModel:** conserva la función privada del fragmento del tutorial. Se incluyen el reinicio explícito de `Loading` y la captura `HttpException` presentes en la solución oficial, además de `IOException`.
- **Pantallas:** se implementan los tres componentes y el `when` del tutorial. Se conserva la vista previa de resultado del starter, sin ejecutar ni generar capturas de las vistas previas. `contentPadding` permanece en la firma como en la referencia oficial.
- **Recursos de carga:** la página menciona `loading_animation.xml`, pero ninguna de las dos ramas de referencia contiene ese archivo. Se conserva `loading_img.xml` y se utiliza exactamente el `Image(painterResource(R.drawable.loading_img))` indicado por la página; no se inventa una animación adicional.
- **Iconos y recursos previos:** se conservan los iconos de lanzamiento y los recursos auxiliares de la plantilla local. Los iconos adaptativos se clasifican por API 26, igual que en la estructura oficial, para soportar el mínimo 24.
- **Coil:** aunque figura entre las dependencias de `repo-starter`, se excluye expresamente porque esta práctica solo obtiene metadatos y muestra un conteo.
- **Pruebas existentes:** se preservan los ejemplos generados por Android Studio y se adapta la aserción del paquete a `com.example.marsphotos`. No se añaden nuevas pruebas ni se ejecutan estos ejemplos.

## Pendiente para la sesión de pruebas gráficas

| Prueba del codelab | Estado |
| --- | --- |
| Sincronización interactiva y apertura en Android Studio | Pendiente; la configuración y resolución de Gradle se comprobaron por su wrapper. |
| Marcador de posición inicial en dispositivo | No ejecutada. Puede revisarse en el commit de preparación. |
| Falta del permiso INTERNET y SecurityException | No ejecutada. El punto intermedio está conservado en el historial. |
| JSON sin procesar visible en Text | No ejecutada. El punto intermedio está conservado en el historial. |
| Cierre sin conexión antes de capturar IOException | No ejecutada; no se registró ni se simuló Logcat. |
| Pantalla de carga | Implementada; observación gráfica pendiente. |
| Cantidad real de fotos recibidas y mensaje final | Implementados; respuesta real y observación en pantalla pendientes. |
| Pantalla de error en modo avión, sin cierre | Implementada; prueba en dispositivo pendiente. |
| Restaurar Internet y relanzar la app | Pendiente. El codelab no solicita recuperación automática ni botón de reintento. |

No se atribuye ningún número de fotografías al servidor en esta sesión. La compilación y el análisis estático no sustituyen las comprobaciones anteriores.
