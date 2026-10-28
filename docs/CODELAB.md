# Análisis y correspondencia con el codelab

Referencia consultada el 28 de octubre de 2026: [Cómo obtener datos de Internet](https://developer.android.com/codelabs/basic-android-kotlin-compose-getting-data-internet?hl=es-419).

Se leyeron los once apartados antes de implementar la conexión. Las referencias oficiales se obtuvieron mediante `git fetch` del [repositorio de Google](https://github.com/google-developer-training/basic-android-kotlin-compose-training-mars-photos):

- `starter`: `00c89fd948581468974c4f73f3219c81ef85947c`.
- `repo-starter`: `ba9470a0471df003e2044cc9a7124ea5ad1047b3`.

## Secuencia estudiada

| Apartado | Instrucciones, archivos y resultado | Comprobación prevista y punto de commit |
| --- | --- | --- |
| 1. Antes de comenzar | Kotlin, Compose, corrutinas y ViewModel; obtener datos y deserializarlos mediante Retrofit y Kotlin Serialization. | Inspeccionar herramientas y proyecto antes de editar. |
| 2. Descripción general | El resultado de esta práctica es un `Text` con la cantidad recibida; la cuadrícula pertenece a otras prácticas. | Revisar que no se incorporen cuadrículas ni descarga de imágenes. |
| 3. App de partida | Usar `starter`. `MainActivity` inicia Compose; `MarsPhotosApp` crea el ViewModel y la barra superior; `HomeScreen` presenta `ResultScreen`; `MarsViewModel` asigna el marcador de posición en `init`. | Preparación del código inicial y compilación. La ejecución visual queda pendiente por indicación del usuario. |
| 4. Servicios web | ViewModel se comunica directamente con el servicio; el repositorio de datos se estudia posteriormente. | Conservar la estructura original sin inyección de dependencias ni capa de repositorios. |
| 5. Retrofit | Añadir Retrofit 2.9.0 y `converter-scalars` 2.9.0 a `app/build.gradle.kts`. GET al servidor oficial y `/photos`. | Resolver dependencias; commit de configuración. |
| 6. Conexión | Crear `network/MarsApiService.kt`: `BASE_URL`, `Retrofit.Builder`, `ScalarsConverterFactory`, `@GET("photos")`, resultado `String`, singleton `MarsApi.retrofitService` con `by lazy`. Después hacer `getPhotos` suspendida y llamarla desde `viewModelScope.launch`, guardando `listResult` en el estado. | Commits separados del servicio y ViewModel. La demostración del fallo por falta de permiso queda pendiente, sin inventar Logcat. |
| 7. Permisos y errores | Declarar únicamente `INTERNET` antes de `application` en el manifiesto; mostrar JSON. Capturar `IOException`. Introducir `sealed interface MarsUiState`, `Success(photos: String)`, `Loading`, `Error`, estado mutable con setter privado. `HomeScreen` selecciona `LoadingScreen`, `ResultScreen` y `ErrorScreen` mediante `when`; usar los recursos de carga y conexión originales. | Commits de permiso, excepción y estados/pantallas. Pruebas de modo avión, ausencia de cierre, carga y recuperación pendientes para una sesión gráfica posterior. |
| 8. JSON | Configurar complemento de serialización compatible con Kotlin. Bibliotecas del codelab: JSON 1.5.1, conversor de Jake Wharton 1.0.0, OkHttp 4.11.0. Crear `network/MarsPhoto.kt`: `@Serializable`, `id: String`, `@SerialName("img_src") val imgSrc: String`. Cambiar Retrofit a `Json.asConverterFactory("application/json".toMediaType())`, `suspend getPhotos(): List<MarsPhoto>`. Mostrar `Success: ${listResult.size} Mars photos retrieved`. | Commits de dependencias, modelo, conversor y mensaje. Compilar; comprobación visual del conteo real pendiente. |
| 9. Solución | Comparar los archivos desarrollados con `repo-starter`, sin copiar la solución como sustituto de las etapas. | Documentar las diferencias justificadas con la página y el entorno. |
| 10. Resumen | GET REST, permiso, inicialización diferida, conversores, arrays JSON y correspondencia de claves mediante `SerialName`. | Revisión final del flujo completo. |
| 11. Más información | Referencias de arquitectura, ViewModel, corrutinas, excepciones, Retrofit y serialización. | No introduce cambios obligatorios ni nuevas funcionalidades. |

## Inspección inicial y conservación

La carpeta contenía una plantilla `Hello Android`, sin Mars Photos ni `.git`. Se guardó íntegramente su contenido versionable en el commit inicial antes de adaptar archivos. No se incluyeron `local.properties`, cachés ni preferencias personales. El código Kotlin y los recursos de Mars Photos proceden de `starter`; se mantiene la configuración Gradle ya instalada y se incorpora la dependencia de ViewModel que utiliza ese código inicial.

La actividad y el tema genéricos fueron sustituidos por los oficiales después de revisarlos; sus versiones anteriores siguen recuperables en el historial. El paquete de la aplicación pasa a `com.example.marsphotos`, como en el proyecto oficial, y `minSdk` a 24, como en `starter`. Se conservan las pruebas generadas por Android Studio; la comprobación del identificador se adapta al paquete de Mars Photos.

## Criterio de fidelidad

La rama `repo-starter` y la página no son idénticas: la rama guarda `MarsPhoto` en `model`, incluye Coil, captura también `HttpException` y usa otras versiones. Se priorizan el paquete `network` y las dependencias solicitadas por la página y el usuario. No se incorpora Coil. La comparación final incorpora la captura `HttpException` y la asignación explícita de `Loading` de la solución oficial. Las adaptaciones de compatibilidad y los resultados reales de compilación se documentan en el README y el registro de validación.

En esta sesión el usuario aplazó todas las pruebas gráficas: no se ejecutará la app ni se probará modo avión, tampoco se afirmará haber observado pantallas o excepciones en un dispositivo.
