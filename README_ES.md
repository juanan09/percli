# 🔍 PerCLI — Asistente de Búsqueda con IA para tu Terminal

<p align="center">
  <strong>Un asistente de IA por línea de comandos que combina Google Gemini con Tavily Search para ofrecer respuestas inteligentes respaldadas por fuentes, directamente desde tu terminal.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/node-%3E%3D18-brightgreen?style=flat-square&logo=node.js" alt="Node.js">
  <img src="https://img.shields.io/badge/licencia-ISC-blue?style=flat-square" alt="Licencia">
  <img src="https://img.shields.io/badge/versión-1.0.0-orange?style=flat-square" alt="Versión">
  <img src="https://img.shields.io/badge/IA-Gemini%202.5%20Flash-blueviolet?style=flat-square&logo=google" alt="Gemini">
  <img src="https://img.shields.io/badge/Búsqueda-Tavily-teal?style=flat-square" alt="Tavily">
  <img src="https://img.shields.io/badge/construido%20con-Genkit-red?style=flat-square&logo=firebase" alt="Genkit">
</p>

---

> 🌐 **[English version](./README.md)**

---

## ✨ Características

- 🤖 **Respuestas Potenciadas por IA** — Utiliza Google Gemini 2.5 Flash para comprensión y generación de lenguaje natural
- 🔎 **Búsqueda Web en Tiempo Real** — Aprovecha la API de Tavily con búsqueda en profundidad avanzada para fundamentar las respuestas con datos actualizados de la web
- 💬 **Modo Chat Interactivo** — Interfaz conversacional con historial de chat mantenido en memoria durante la sesión
- 🛠️ **Sistema de Herramientas Genkit** — La búsqueda se expone como una herramienta de Genkit, permitiendo que la IA decida autónomamente cuándo buscar en la web
- 📑 **Citas de Fuentes** — Las respuestas incluyen citas numeradas y una sección dedicada de Fuentes con URLs
- 🎨 **Interfaz de Terminal Elegante** — Salida con colores, spinners y respuestas formateadas usando Chalk y Ora
- 🔧 **Herramientas de Desarrollo Genkit** — Construido sobre [Firebase Genkit](https://firebase.google.com/docs/genkit) para observabilidad, trazas y depuración

## 📋 Requisitos Previos

- [Node.js](https://nodejs.org/) **v18 o superior**
- Una [Tavily API Key](https://tavily.com) (para búsqueda web)
- Una [Google AI API Key](https://aistudio.google.com/app/apikey) (para el modelo Gemini)

## 🚀 Inicio Rápido

### 1. Clonar el repositorio

```bash
git clone https://github.com/juanan09/percli.git
cd percli
```

### 2. Instalar dependencias

```bash
npm install
```

### 3. Configurar las variables de entorno

Copia el archivo de ejemplo y añade tus claves API:

```bash
cp .env.example .env
```

Edita `.env` con tus claves:

```env
# Clave API de Tavily - Consíguela en https://tavily.com
TAVILY_API_KEY=tu_clave_api_tavily_aqui

# Clave API de Google AI - Consíguela en https://aistudio.google.com/app/apikey
GOOGLE_API_KEY=tu_clave_api_google_aqui
```

### 4. Ejecutar la aplicación

```bash
# Modo producción
npm start

# Modo desarrollo (con Genkit Dev UI para trazas y depuración)
npm run dev
```

## 🎮 Uso

Al ejecutar la aplicación, verás un prompt interactivo:

```
╔════════════════════════════════════════════════════╗
║   Welcome to Percli CLI - Interactive Mode     ║
╚════════════════════════════════════════════════════╝
Type your questions and get AI-powered answers with sources!
Chat history is maintained during this session.
Commands: exit, quit, or press Ctrl+C to leave

💬 Ask a question (or type "exit" to quit):
```

Simplemente escribe tu pregunta y pulsa Enter. La IA:

1. Analizará tu consulta
2. Buscará automáticamente en la web si necesita información actualizada
3. Sintetizará información de múltiples fuentes
4. Proporcionará una respuesta completa y bien estructurada con citas de fuentes

**Comandos:**

| Comando         | Descripción            |
| --------------- | ---------------------- |
| `exit` / `quit` | Salir de la aplicación |
| `Ctrl+C`        | Forzar salida          |

> **💡 Consejo:** El historial del chat se mantiene durante tu sesión, así que la IA recuerda preguntas anteriores y puede construir sobre el contexto previo.

## 🏗️ Arquitectura

PerCLI está construido sobre una arquitectura de **agente aumentado con herramientas** usando Firebase Genkit:

```
┌──────────────────────────────────────────────────────────┐
│                    index.js (Bucle CLI)                   │
│  ┌─────────────┐  ┌──────────┐  ┌─────────────────────┐ │
│  │   readline   │  │  chalk   │  │        ora          │ │
│  │  (E/S user)  │  │(colores) │  │    (spinners)       │ │
│  └──────┬───────┘  └──────────┘  └─────────────────────┘ │
│         │                                                 │
│         ▼                                                 │
│  ┌─────────────────────────────────────────────────────┐  │
│  │            agent.js (Agente de Chat)                │  │
│  │  ┌──────────────────┐  ┌─────────────────────────┐  │  │
│  │  │   Genkit Prompt  │  │  Herramienta searchWeb │  │  │
│  │  │  (prompt del     │  │  (la IA decide cuándo  │  │  │
│  │  │   sistema + cfg) │  │   invocar la búsqueda) │  │  │
│  │  └──────────────────┘  └────────────┬────────────┘  │  │
│  └─────────────────────────────────────┤───────────────┘  │
│                                        │                  │
│  ┌─────────────────────────────────────▼───────────────┐  │
│  │          search.js (Integración con Tavily)         │  │
│  │  ┌──────────────────────────────────────────────┐   │  │
│  │  │  Búsqueda avanzada · Máx N resultados · Resp │   │  │
│  │  └──────────────────────────────────────────────┘   │  │
│  └─────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

### Cómo Funciona

1. **`index.js`** — Punto de entrada. Configura la instancia de Genkit AI, el cliente de Tavily, y el bucle interactivo basado en readline. Valida las variables de entorno y gestiona el ciclo de vida de la CLI.
2. **`src/agent.js`** — Define el agente de chat con IA. Crea un prompt de Genkit con instrucciones del sistema que indican a la IA cómo responder, y registra una herramienta `searchWeb` que la IA puede invocar autónomamente. Usa esquemas Zod para validación de entrada/salida.
3. **`src/search.js`** — Envuelve el cliente de la API de Tavily. Realiza búsquedas web avanzadas con límites de resultados configurables y devuelve resultados estructurados (título, URL, contenido).

## 📁 Estructura del Proyecto

```
percli/
├── index.js              # Punto de entrada — bucle CLI interactivo e inicialización
├── src/
│   ├── agent.js          # Configuración del agente — prompt, modelo y registro de herramientas
│   └── search.js         # Wrapper de búsqueda web Tavily con búsqueda en profundidad avanzada
├── .env.example          # Plantilla de variables de entorno
├── .gitignore            # Reglas de ignorar de Git
├── package.json          # Dependencias, scripts y metadatos del proyecto
├── README.md             # Documentación en inglés
└── README_ES.md          # Este archivo (español)
```

## 📦 Dependencias

### Producción

| Paquete                                                                          | Versión | Descripción                                                           |
| -------------------------------------------------------------------------------- | ------- | --------------------------------------------------------------------- |
| [genkit](https://www.npmjs.com/package/genkit)                                   | ^1.29.0 | Framework Firebase Genkit para construir aplicaciones con IA          |
| [@genkit-ai/google-genai](https://www.npmjs.com/package/@genkit-ai/google-genai) | ^1.29.0 | Plugin de Google Gemini para Genkit                                   |
| [@genkit-ai/googleai](https://www.npmjs.com/package/@genkit-ai/googleai)         | ^1.28.0 | Integración de Google AI para Genkit                                  |
| [@tavily/core](https://www.npmjs.com/package/@tavily/core)                       | 0.7.2   | Cliente de la API de búsqueda Tavily para búsqueda web en tiempo real |
| [chalk](https://www.npmjs.com/package/chalk)                                     | ^5.6.2  | Estilizado de cadenas en la terminal con colores y formato            |
| [ora](https://www.npmjs.com/package/ora)                                         | ^9.3.0  | Spinners elegantes para estados de carga en la terminal               |
| [dotenv](https://www.npmjs.com/package/dotenv)                                   | ^17.3.1 | Carga variables de entorno desde archivos `.env`                      |

### Desarrollo

| Paquete                                                | Versión | Descripción                                                 |
| ------------------------------------------------------ | ------- | ----------------------------------------------------------- |
| [genkit-cli](https://www.npmjs.com/package/genkit-cli) | ^1.29.0 | CLI de Genkit para la UI de desarrollo, trazas y depuración |

## 📜 Scripts

| Script  | Comando       | Descripción                                                                                              |
| ------- | ------------- | -------------------------------------------------------------------------------------------------------- |
| `start` | `npm start`   | Ejecuta la CLI en modo producción (`node index.js`)                                                      |
| `dev`   | `npm run dev` | Ejecuta con la UI de desarrollo de Genkit para trazas y depuración (`npx genkit start -- node index.js`) |
| `test`  | `npm test`    | Placeholder para tests (aún no implementado)                                                             |

## 🔧 Configuración

Toda la configuración se realiza mediante variables de entorno en el archivo `.env`:

| Variable         | Requerida | Descripción                                                                                      |
| ---------------- | --------- | ------------------------------------------------------------------------------------------------ |
| `TAVILY_API_KEY` | ✅         | Clave API para búsqueda web de Tavily — consíguela en [tavily.com](https://tavily.com)           |
| `GOOGLE_API_KEY` | ✅         | Clave API para Google Gemini — consíguela en [AI Studio](https://aistudio.google.com/app/apikey) |

## 🧠 Modelo de IA

PerCLI utiliza **Google Gemini 2.5 Flash** como modelo de lenguaje subyacente, configurado a través de Genkit. El modelo está instruido para:

- Proporcionar respuestas completas y factuales basadas en resultados de búsqueda web
- Sintetizar información de múltiples fuentes
- Usar citas numeradas `[1]`, `[2]`, etc. para referenciar fuentes
- Incluir una sección de "Fuentes" al final de cada respuesta con URLs de referencia
- Usar formato markdown para mejor legibilidad
- Decidir autónomamente cuándo usar la herramienta de búsqueda web

## 🛡️ Manejo de Errores

PerCLI incluye un manejo de errores robusto:

- **Claves API faltantes** — Mensajes de error claros con consejos accionables si `TAVILY_API_KEY` o `GOOGLE_API_KEY` no están configuradas
- **Fallos en la búsqueda** — Los errores de búsqueda de Tavily se capturan y se muestran con mensajes descriptivos
- **Errores en tiempo de ejecución** — Los errores por consulta se capturan sin cerrar la sesión, permitiendo continuar la interacción
- **Cierre elegante** — Salida limpia con mensaje de despedida al usar `exit`, `quit` o `Ctrl+C`

## 🤝 Contribuir

1. Haz un fork del repositorio
2. Crea tu rama de feature (`git checkout -b feature/caracteristica-genial`)
3. Haz commit de tus cambios (`git commit -m 'Añadir característica genial'`)
4. Haz push a la rama (`git push origin feature/caracteristica-genial`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está licenciado bajo la **Licencia ISC**.

---

<p align="center">
  Hecho con ❤️ usando <a href="https://firebase.google.com/docs/genkit">Genkit</a>, <a href="https://ai.google.dev/">Google Gemini</a> y <a href="https://tavily.com">Tavily</a>
</p>
