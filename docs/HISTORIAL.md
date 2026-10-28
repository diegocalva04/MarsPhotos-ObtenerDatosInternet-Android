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

El commit que incorpora este registro, el README y la validación es la etapa final de documentación. Su hash puede consultarse en `git log -1`; no se inserta su propio hash dentro del archivo porque eso modificaría el contenido que identifica ese mismo commit.

Para consultar todos los hashes completos, mensajes, fechas y cuerpos:

```powershell
git log --reverse --format=fuller main
```

Los puntos intermedios conservan deliberadamente la secuencia didáctica. El servicio inicialmente devuelve String y todavía no declara permiso de Internet; la sustitución del conversor y la adaptación del mensaje se registran en etapas consecutivas. El código final es el que se compila y publica en `main`. No se probaron gráficamente estos estados intermedios.
