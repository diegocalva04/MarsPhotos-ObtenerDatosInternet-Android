# Historial de desarrollo por etapas

Todos los commits son reales y sus cuerpos explicativos están redactados en español. No se alteraron fechas ni se crearon commits vacíos. Cada etapa se revisó mediante `git diff` y `git status`.

| Hash | Etapa y mensaje | Cambios principales |
| --- | --- | --- |
| `fa1b59e` | `chore: conservar plantilla Android existente antes de preparar Mars Photos` | Guarda la plantilla local original antes de modificarla. |
| `53642f1` | `chore: preparar proyecto Mars Photos desde el código inicial de Google` | Incorpora Kotlin y recursos de `starter`, licencia, paquete oficial, ViewModel y análisis de los once apartados. Conserva herramientas locales y adapta iconos al mínimo 24. |
| `1a8e4a4` | `build: incorporar Retrofit y conversor escalar para solicitudes HTTP` | Añade Retrofit 2.9.0 y `converter-scalars` 2.9.0 para la fase String. |
| `526b443` | `feat: crear servicio MarsApiService y configurar cliente Retrofit` | Crea cliente, URL base, GET photos, singleton e inicialización diferida. |
| `eed9c47` | `feat: conectar MarsViewModel con el servicio REST mediante corrutinas` | Convierte `getPhotos` en suspend y actualiza el estado desde `viewModelScope.launch`. |
| `d55a83d` | `feat: habilitar acceso a Internet y recuperar respuesta JSON de Mars Photos` | Declara exclusivamente el permiso INTERNET. La ejecución visual del JSON queda pendiente. |
| `e31725d` | `fix: controlar excepciones IOException durante solicitudes de red` | Incorpora el try-catch intermedio del tutorial. |
| `5028b0d` | `feat: implementar estados Loading Success y Error con sus pantallas Compose` | Define la interfaz sellada y el estado mutable; agrega el when y los componentes con recursos oficiales. |
| `21dbe82` | `build: configurar Kotlin Serialization y conversor JSON de Retrofit` | Configura complemento compatible, JSON, conversor de Jake Wharton y OkHttp; retira la dependencia escalar. |
| `4a771ae` | `feat: crear modelo MarsPhoto con anotaciones de serialización JSON` | Crea `network/MarsPhoto`, `@Serializable` y el mapeo de `img_src`. |
| `88564e8` | `refactor: reemplazar conversor escalar por deserialización Kotlinx Serialization` | Usa Json, el tipo application/json y `List<MarsPhoto>`. |
| `731f89e` | `feat: mostrar cantidad de fotografías recuperadas desde el servicio REST` | Forma el mensaje exacto con `listResult.size`. |
| `ea309a4` | `fix: alinear manejo de respuestas HTTP con la solución oficial` | Incorpora Loading explícito y captura HttpException tras comparar con `repo-starter`. |
| `2c98bc8` | `docs: documentar configuración funcionamiento e historial de Mars Photos` | Registra configuración, análisis, historial y validaciones iniciales, con pruebas gráficas aplazadas. |
| `352605c` | `fix: corregir contraste de la barra de estado con el tema oficial` | Tras revisar el dispositivo, adapta Theme.kt exactamente a repo-starter y comprueba que los iconos se leen sobre fondo claro. |

El commit que actualiza este registro y añade las evidencias gráficas es la etapa actual de documentación. Su hash puede consultarse en `git log -1`; no se inserta su propio hash dentro del archivo porque eso modificaría el contenido que identifica ese mismo commit.

Para consultar todos los hashes completos, mensajes, fechas y cuerpos:

```powershell
git log --reverse --format=fuller main
```

Los puntos intermedios conservan deliberadamente la secuencia didáctica. El servicio inicialmente devuelve String y todavía no declara permiso de Internet; la sustitución del conversor y la adaptación del mensaje se registran en etapas consecutivas. El código final es el que se compila y publica en `main`. Se probaron gráficamente sus estados Loading, Success y Error y la recuperación al relanzar; las demostraciones de las versiones intermedias permanecen sin ejecutar.

## Commits de la ampliación de repositorio y DI

Se clonó `diegocalva04/MarsPhotos-ObtenerDatosInternet-Android` en una carpeta nueva y se creó la rama local `codelab-repositorio-di` desde `main` (`36c7414`). El repositorio remoto no se modificó.

| Hash | Mensaje | Cambios |
| --- | --- | --- |
| `0350d9c` | `feat: agregar repositorio para las fotografías de Marte` | Interfaz y repositorio de red; ViewModel llama al repositorio. |
| `3c7816a` | `feat: agregar contenedor de dependencias y configurar Retrofit` | AppContainer, DefaultAppContainer, servicio Retrofit por constructor y retiro del singleton. |
| `4c8862d` | `feat: vincular el contenedor de dependencias con la aplicación` | Inicialización del contenedor y registro en AndroidManifest. |
| `7aff621` | `feat: inyectar el repositorio en MarsViewModel` | Fábrica de ViewModel y conexión desde Compose. |
| `d9bade2` | `test: configurar dependencias para pruebas locales` | JUnit 4.13.2 y kotlinx-coroutines-test 1.7.1. |
| `af1bc73` | `test: crear datos y servicio falsos para las pruebas` | FakeDataSource y FakeMarsApiService. |
| `e6ec3ac` | `test: verificar la recuperación de fotos desde el repositorio` | Prueba de NetworkMarsPhotosRepository con runTest. |
| `00dfaef` | `test: validar el estado Success de MarsViewModel` | Repositorio falso, TestDispatcherRule y prueba de estado Success. |

El historial anterior de la práctica REST permanece sin reescritura. La ampliación se mantiene como commits locales en `codelab-repositorio-di` hasta que se decida su publicación.
