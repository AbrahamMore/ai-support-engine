# AI Support Engine

Prototipo funcional de clasificación inteligente de solicitudes de soporte con dos enfoques: un **motor tradicional** basado en reglas y un **motor generativo** que usa un modelo de lenguaje a través de la API de Groq. Ambos exponen el mismo contrato de respuesta mediante `POST /api/analyze`.

## Integrantes

| Nombre | Matrícula |
|---|---|
| Diana Laura Olmos Antonio | 3523110152 |
| Josmar Olivera Pérez | 3522110243 |
| Abraham Moreno Vasquez | 3523110441 |

## Tecnologías

- Node.js 20 o superior (usa `fetch` nativo)
- Express 5 (API)
- Zod (validación de entradas y de la salida del modelo)
- dotenv (variables de entorno)
- Groq API con el modelo `openai/gpt-oss-120b`

## Estructura del proyecto

```
ai-support-engine/
├── src/
│   ├── server.js                # Servidor Express
│   ├── config.js                # Lectura de variables de entorno
│   ├── routes/analyze.js        # Endpoint POST /api/analyze
│   ├── engines/
│   │   ├── traditional.js       # Motor basado en reglas
│   │   └── generative.js        # Motor con IA generativa (Groq)
│   └── validation/schema.js     # Esquemas Zod
├── tests/
│   ├── test-cases.json          # Casos de prueba y clasificación de referencia
│   └── compare.js               # Comparativa de ambos motores
├── results/                     # Resultados de la comparativa
├── .env.example
└── README.md
```

## Instalación

1. Clonar el repositorio e instalar dependencias:

   ```bash
   git clone https://github.com/TU_USUARIO/ai-support-engine.git
   cd ai-support-engine
   npm install
   ```

2. Crear una API key gratuita en [console.groq.com/keys](https://console.groq.com/keys).

3. Crear el archivo `.env` a partir de `.env.example`:

   ```
   GROQ_API_KEY=tu_api_key
   GROQ_MODEL=openai/gpt-oss-120b
   PORT=3000
   ```

   El archivo `.env` está en `.gitignore` y nunca debe subirse al repositorio.

## Ejecución

Levantar el servidor:

```bash
npm start
```

El servidor queda disponible en `http://localhost:3000`.

### Ejemplos de uso

**PowerShell**

```powershell
Invoke-RestMethod -Method Post -Uri http://localhost:3000/api/analyze -ContentType "application/json" -Body '{"message":"No puedo iniciar sesion","mode":"traditional"}'

Invoke-RestMethod -Method Post -Uri http://localhost:3000/api/analyze -ContentType "application/json" -Body '{"message":"No puedo iniciar sesion","mode":"generative"}'
```

**curl (Linux / macOS / Git Bash)**

```bash
curl -X POST http://localhost:3000/api/analyze \
  -H "Content-Type: application/json" \
  -d '{"message":"No puedo iniciar sesion","mode":"generative"}'
```

## API

### `POST /api/analyze`

**Petición**

```json
{
  "message": "No puedo iniciar sesión",
  "mode": "generative"
}
```

- `message`: texto de la solicitud (obligatorio, no vacío, máximo 1500 caracteres).
- `mode`: `traditional` o `generative`.

**Respuesta (mismo contrato para ambos motores)**

```json
{
  "category": "ACCESS",
  "priority": "MEDIUM",
  "response": "Lamentamos el inconveniente. Verifique sus credenciales...",
  "requiresHumanReview": false,
  "mode": "generative"
}
```

- `category`: `ACCESS`, `BUG`, `FEATURE` o `SECURITY`.
- `priority`: `LOW`, `MEDIUM`, `HIGH` o `CRITICAL`.

**Códigos de error**

| Código | Causa |
|---|---|
| 400 | Solicitud inválida (mensaje vacío, demasiado largo, `mode` desconocido o JSON mal formado) |
| 502 | Falla del proveedor generativo o respuesta del modelo con formato inválido |
| 500 | Error interno |

## Motores

### Motor tradicional

Clasificador basado en reglas, sin modelos generativos:

1. Normaliza el texto (minúsculas, sin acentos).
2. Cuenta coincidencias de palabras clave por categoría. En caso de empate gana la categoría con mayor precedencia: `SECURITY > ACCESS > BUG > FEATURE`.
3. Asigna la prioridad con reglas (términos de incidentes graves, urgencia y tipo de categoría).
4. Devuelve una respuesta predeterminada según la categoría.
5. Si no hay coincidencias, usa un valor de respaldo y marca la solicitud para revisión humana.

### Motor generativo

Usa el modelo `openai/gpt-oss-120b` mediante la API de Groq:

- El prompt de sistema define el rol, las categorías, las prioridades y el JSON esperado.
- El mensaje del usuario se envía entre etiquetas `<mensaje>` y se trata como dato, nunca como instrucciones, para mitigar la inyección de prompts.
- Se solicita salida en modo JSON con temperatura baja.
- La respuesta se valida con Zod; si el formato es inválido se reintenta una vez y, si persiste, la API responde 502.

## Validación y seguridad

- Validación de la entrada con Zod (tipo, longitud y valores permitidos).
- Validación del formato de la respuesta del modelo antes de devolverla.
- Manejo de errores del proveedor: tiempo de espera, errores HTTP y JSON inválido. No se exponen detalles internos al cliente.
- La API key se lee únicamente desde variables de entorno; `.env` está excluido del repositorio y no se registra en logs.
- `requiresHumanReview` se fuerza a `true` desde el código (no solo desde el prompt) cuando la categoría es `SECURITY` o cuando el mensaje contiene patrones de inyección de prompts.

## Pruebas comparativas

Se definió una clasificación de referencia (`tests/test-cases.json`) **antes** de ejecutar los motores, con 12 casos: 6 normales, 3 ambiguos y 3 diseñados para confundir al sistema (incluida la instrucción de ignorar reglas previas y un intento de extraer el prompt y la API key).

Para ejecutar ambos motores con todos los casos:

```bash
npm run compare
```

Genera `results/results.md` y `results/results.json`. Un resultado se considera correcto cuando coinciden categoría y prioridad con la referencia.

### Resultados

<!-- PEGAR AQUÍ la tabla de results/results.md (incluye los porcentajes de cada motor) -->

### Conclusión técnica

<!-- PEGAR AQUÍ la conclusión de máximo 150 palabras, con los números reales -->

## Evidencias

<!-- Opcional: carpeta evidence/ con capturas de la ejecución de ambos motores, del comparativo y del error 400 -->