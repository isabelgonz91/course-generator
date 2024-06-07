# Generador de Material de Formación

Este repositorio contiene un conjunto de scripts en Python diseñados para generar material de formación. Utiliza el modelo de lenguaje GPT-4 de OpenAI para generar el contenido del curso que desees, solo tienes que modificar el prompt para indicarlo. En este caso en el ejemplo lo hemos hecho de fotografía. El proceso incluye la creación de un índice del curso, la expansión de cada punto del índice en capítulos completos, y la extensión de estos capítulos en secciones más detalladas. A continuación se detallan los pasos y componentes del script.

## Requisitos

- Python 3.7 o superior
- Archivo `.env` con la clave de API de OpenAI (`OPENAI_API_KEY`)

## Instalación

1. Clona este repositorio.
2. Instala los paquetes requeridos utilizando `pip`:

    ```bash
    pip install -r requirements.txt
    ```

3. Crea un archivo `.env` en el directorio raíz del proyecto y añade tu clave de API de OpenAI:

    ```
    OPENAI_API_KEY=tu_clave_de_api
    ```

## Uso

### Estructura del Script

El script principal se compone de las siguientes secciones:

1. **Carga de Variables de Entorno:**

    ```python
    from dotenv import load_dotenv, find_dotenv
    load_dotenv(find_dotenv())
    os.environ['OPENAI_API_KEY'] = os.getenv("OPENAI_API_KEY")
    ```

    Esta sección carga las variables de entorno desde un archivo `.env` para configurar la clave de API de OpenAI.

2. **Función para Guardar Texto en Archivo:**

    ```python
    def save_to_file(filename, content):
        with open(filename, 'w', encoding='utf-8') as file:
            file.write(content)
    ```

    Esta función guarda el contenido generado en archivos de texto.

3. **Inicialización del Modelo de Lenguaje:**

    ```python
    from langchain.llms import OpenAI
    from langchain_openai import ChatOpenAI
    llm = ChatOpenAI(model="gpt-4")
    ```

    Esta sección inicializa el modelo GPT-4 de OpenAI para generar el contenido del curso.

4. **Generación del Índice del Curso:**

    ```python
    def generar_indice_curso():
        indice_prompt = "Actúa como un fotógrafo especializado en desarrollar contenido de formación y genera el índice para un curso de fotografía dirigido a pequeños negocios que quieren aprender a hacer buenas fotografías por ellos mismos en un formato Do it yourself. Que el índice incluya mínimo 16 puntos."
        messages = [{"role": "user", "content": indice_prompt}]
        response = llm.invoke(messages)
        return response.content.strip()
    ```

    Esta función genera el índice del curso utilizando un prompt específico.

5. **Extensión de Puntos del Índice en Capítulos:**

    ```python
    def extender_punto(punto):
        extension_prompt = f"Desarrolla el siguiente punto del índice en un capítulo completo del curso de fotografía DIY: {punto}"
        messages = [{"role": "user", "content": extension_prompt}]
        response = llm.invoke(messages)
        return response.content.strip()
    ```

    Esta función expande cada punto del índice en un capítulo completo.

6. **Extensión de Capítulos en Secciones Detalladas:**

    ```python
    def extender_capitulo(capitulo):
        subpunto_prompt = f"Desarrolla más a fondo el siguiente capítulo del curso de fotografía DIY: {capitulo}"
        messages = [{"role": "user", "content": subpunto_prompt}]
        response = llm.invoke(messages)
        return response.content.strip()
    ```

    Esta función extiende cada capítulo en secciones más detalladas.

### Ejecución del Script

1. **Generación del Índice:**

    ```python
    indice = generar_indice_curso()
    save_to_file("indice_curso.txt", indice)
    ```

    Genera y guarda el índice del curso en un archivo `indice_curso.txt`.

2. **Extensión de los Puntos del Índice:**

    ```python
    puntos_indice = indice.split("\n")
    capitulos = []
    for punto in puntos_indice:
        capitulo = extender_punto(punto)
        capitulos.append(capitulo)
        save_to_file(f"capitulo_{puntos_indice.index(punto) + 1}.txt", capitulo)
    ```

    Expande cada punto del índice en capítulos y guarda cada capítulo en archivos separados.

3. **Extensión de Capítulos en Secciones Detalladas:**

    ```python
    material_formacion_completo = ""
    for capitulo in capitulos:
        seccion = extender_capitulo(capitulo)
        material_formacion_completo += seccion + "\n\n"
    save_to_file("material_formacion_completo.txt", material_formacion_completo)
    ```

    Extiende cada capítulo en secciones más detalladas y guarda todo el material de formación en un archivo `material_formacion_completo.txt`.

### Resultado

El script genera y guarda los siguientes archivos:

- `indice_curso.txt`: Contiene el índice del curso.
- `capitulo_<n>.txt`: Archivos separados para cada capítulo del curso.
- `material_formacion_completo.txt`: Contiene todo el material de formación completo y detallado.

### Nota Final

Este script automatiza la creación de material educativo utilizando la capacidad de generación de texto del modelo GPT-4 de OpenAI, lo que facilita la producción de contenido formativo de alta calidad.
