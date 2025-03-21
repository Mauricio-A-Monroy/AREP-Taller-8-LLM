# LLM Chain con LangChain y OpenAI

## Descripción del Proyecto
Este proyecto es una implementación del tutorial de **LLM Chain** de LangChain. Se centra en la creación de un pipeline para generar respuestas en base a prompts estructurados. En este caso, el modelo traduce texto del inglés a un idioma especificado por el usuario.

## Arquitectura del Proyecto
El sistema sigue una arquitectura basada en:

1. **Inicialización del Modelo**: Se utiliza `init_chat_model` para cargar un modelo de lenguaje.
2. **Generación de Prompt**: Se usa `ChatPromptTemplate` para estructurar los mensajes de entrada.
3. **Invocación del Modelo**: Se pasa el prompt al modelo y se obtiene la traducción.

### Diagrama de Flujo del Proceso

![image](https://github.com/user-attachments/assets/95aa1436-a0a2-45ec-8a90-ca696beb4209)


## Requisitos de Instalación
Antes de ejecutar el código, asegúrate de tener instaladas las siguientes dependencias:

```bash
pip install langchain langchain-core openai
```

## Uso
1. Configura las claves de API:
   - **LANGSMITH_API_KEY** para habilitar LangSmith tracing.
   - **OPENAI_API_KEY** para conectarse al modelo de OpenAI.

2. Ejecuta el siguiente código en Python:

```python
import getpass
import os

os.environ["LANGSMITH_TRACING"] = "true"
os.environ["LANGSMITH_API_KEY"] = getpass.getpass()

if not os.environ.get("OPENAI_API_KEY"):
    os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter API key for OpenAI: ")

from langchain.chat_models import init_chat_model
from langchain_core.prompts import ChatPromptTemplate

model = init_chat_model("gpt-4o-mini", model_provider="openai")

system_template = "Translate the following from English into {language}"

prompt_template = ChatPromptTemplate.from_messages([
    ("system", system_template),
    ("user", "{text}")
])

prompt = prompt_template.invoke({"language": "Italian", "text": "hi!"})
print(prompt.to_messages())

response = model.invoke(prompt)
print(response.content)
```

### Explicación del Código
1. **Configuración de Entorno:** 
   - Se activa el rastreo de LangSmith (`LANGSMITH_TRACING = "true"`).
   - Se solicita al usuario la clave API de LangSmith y OpenAI si no están definidas en las variables de entorno.

2. **Inicialización del Modelo:**
   - Se carga el modelo `gpt-4o-mini` de OpenAI con `init_chat_model`.

3. **Creación del Prompt:**
   - Se define un sistema de mensajes en `ChatPromptTemplate`, donde:
     - Un mensaje del sistema (`system`) especifica la tarea de traducir de inglés al idioma indicado.
     - Un mensaje del usuario (`user`) contiene el texto a traducir.

4. **Generación de la Traducción:**
   - Se invoca el `prompt_template` con los valores de idioma (`Italian`) y texto (`hi!`).
   - Se imprime el prompt generado.
   - Se pasa el prompt al modelo de lenguaje, que devuelve la traducción.
   - Se imprime la respuesta del modelo.

## Ejemplo de Salida

```plaintext
[{'role': 'system', 'content': 'Translate the following from English into Italian'},
 {'role': 'user', 'content': 'hi!'}]
Ciao!
```

![image](https://github.com/user-attachments/assets/52299aab-a401-435f-a233-7ee3c9a331cb)


Conclusiones

- Facilidad de Uso: LangChain simplifica la interacción con modelos de lenguaje, permitiendo construir flujos conversacionales personalizados con poco código.

- Flexibilidad: Se pueden modificar los prompts para adaptar la aplicación a diferentes tareas como resúmenes, generación de texto o traducción.

- Extensibilidad: La implementación puede ampliarse agregando almacenamiento de contexto o integración con otras herramientas como bases de datos vectoriales.

- Consideraciones de Costos: El uso de la API de OpenAI implica costos, por lo que es recomendable monitorear su uso en proyectos grandes.

Este proyecto puede servir como base para desarrollar sistemas de traducción más avanzados o asistentes conversacionales especializados.

