# Mars Photos — Cómo obtener datos de Internet

Aplicación Android en Kotlin y Jetpack Compose desarrollada siguiendo el [codelab oficial de Google](https://developer.android.com/codelabs/basic-android-kotlin-compose-getting-data-internet?hl=es-419). Consulta un servicio REST, convierte su respuesta JSON en objetos Kotlin y muestra la cantidad de fotografías recuperadas.

El resultado de esta práctica es un mensaje `Success: <cantidad> Mars photos retrieved`, acompañado por estados de carga y error. La cantidad se calcula con el tamaño de la lista recibida. No se implementan la cuadrícula, la descarga visual de fotografías ni funcionalidades de los codelabs posteriores.

## Tecnologías

| Componente | Configuración |
| --- | --- |
| Lenguaje e interfaz | Kotlin 2.2.10 y Jetpack Compose; BOM declarado 2026.02.01 |
| Herramientas existentes | Android Gradle Plugin 9.3.3 y Gradle Wrapper 9.5.0 |
| Android | `minSdk = 24`, `compileSdk = 37`, `targetSdk = 37` |
| Estado y corrutinas | ViewModel, `mutableStateOf`, `viewModelScope.launch`; Lifecycle 2.11.0 |
| HTTP | Retrofit 2.9.0 y OkHttp 4.11.0 |
| Conversor | `retrofit2-kotlinx-serialization-converter:1.0.0` de Jake Wharton |
| JSON | `kotlinx-serialization-json:1.5.1` declarado; Gradle resuelve 1.7.3 por restricciones transitivas de AndroidX |
| Complemento de serialización | 2.2.10, alineado con Kotlin del proyecto |

Se conservó la configuración moderna de la carpeta original. El complemento 1.8.10 mostrado por el codelab se adapta a 2.2.10 para corresponder al compilador local; no se reemplazan las tecnologías del tutorial ni se fuerzan versiones transitivas inferiores. Las bibliotecas de interfaz ya existentes pueden seleccionar versiones superiores a las del BOM por sus propias restricciones.

## Abrir y compilar

1. Abrir esta carpeta directamente en Android Studio y permitir la sincronización de Gradle.
2. Tener instalado Android SDK 37. La configuración de Gradle solicita un JDK 25 para su proceso de compilación; puede utilizarse el incluido con Android Studio en este entorno. El archivo `gradle/gradle-daemon-jvm.properties` conserva la configuración de aprovisionamiento existente.
3. Configurar la ruta del SDK mediante Android Studio o un `local.properties` propio. Este archivo no se versiona.
4. Compilar desde Android Studio o, en PowerShell, ejecutar:

```powershell
.\gradlew.bat :app:assembleDebug
```

El APK se genera en `app/build/outputs/apk/debug/app-debug.apk`. No se incluye en Git. Para la ejecución posterior se necesita un dispositivo o emulador con Android 7.0/API 24 o superior y conexión a Internet.

## Funcionamiento

La única solicitud es `GET https://android-kotlin-fun-mars-server.appspot.com/photos`. `MarsApiService` declara `suspend fun getPhotos(): List<MarsPhoto>` y el objeto `MarsApi` expone `retrofitService` con inicialización diferida mediante `by lazy`.

`network/MarsPhoto.kt` tiene `@Serializable`, un `id: String` y `imgSrc: String`. `@SerialName("img_src")` relaciona el nombre Kotlin con la clave JSON. Retrofit usa `Json.asConverterFactory("application/json".toMediaType())` para construir la lista automáticamente.

Al crearse, `MarsViewModel` inicia la solicitud en `viewModelScope.launch`. `MarsUiState` es una interfaz sellada con `Loading`, `Success(photos: String)` y `Error`. `HomeScreen` selecciona la pantalla mediante un `when` exhaustivo. El estado de éxito contiene `listResult.size`, sin números fijos. Se captura `IOException` para fallos de conexión y `HttpException`, como en la solución oficial, para respuestas HTTP no satisfactorias. Solo se declara el permiso `INTERNET`.

## Validación realizada

- Compilación del código inicial, de la fase escalar y de la implementación final: correctas.
- `:app:assembleDebug :app:lintDebug` sobre el código final: `BUILD SUCCESSFUL`; Lint informa **0 errores y 25 advertencias**.
- Comparación del servicio, modelo, ViewModel, pantallas, recursos y configuración con las ramas oficiales `starter` y `repo-starter`.
- Revisión del historial, diferencias e inclusión de archivos; configuraciones personales y resultados de compilación excluidos de Git.
- Pruebas gráficas en Xiaomi 23100RN82L con Android 15: carga, éxito con 838 fotos reales, error en modo avión sin Wi-Fi, ausencia de cierre fatal, recuperación al relanzar y salida/reapertura con Atrás.
- Corrección del contraste de la barra de estado aplicando exactamente el tema de la solución oficial `repo-starter`.

Las pruebas gráficas se aplazaron durante la primera sesión y se completaron posteriormente con autorización del usuario. Las [capturas y resultados](docs/PRUEBAS_GRAFICAS.md) corresponden a la ejecución real del APK corregido. Los 838 registros son la respuesta observada durante la prueba; la aplicación obtiene siempre el conteo de la lista recibida. No se ejecutaron pruebas unitarias o instrumentadas; se conservaron las generadas en la plantilla original.

El [registro de validación](docs/VALIDACION.md) distingue las comprobaciones iniciales, las pruebas gráficas posteriores y las demostraciones intermedias del tutorial que no se ejecutaron. El cambio local previo del wrapper a Gradle 9.8.2 se conservó fuera de estos commits; el APK probado se compiló con Gradle 9.5.0, como la configuración publicada.

## Referencias e historial

El código de partida procede del [repositorio oficial de Google, rama starter](https://github.com/google-developer-training/basic-android-kotlin-compose-training-mars-photos/tree/starter), y la comparación final utiliza [repo-starter](https://github.com/google-developer-training/basic-android-kotlin-compose-training-mars-photos/tree/repo-starter). Se conserva su licencia Apache 2.0 en `LICENSE`.

La carpeta original contenía una plantilla Android sin Git. El primer commit conserva esa plantilla y los siguientes registran el desarrollo progresivo en español. Consulta el [historial por etapas](docs/HISTORIAL.md), el [análisis de los once apartados](docs/CODELAB.md) y `git log --reverse --format=fuller` para ver el historial completo con cuerpos explicativos.
