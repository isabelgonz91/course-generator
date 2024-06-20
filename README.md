# Educational Material Generator

This repository contains a set of Python scripts designed to generate educational material. It uses OpenAI's GPT-4 language model to create the desired course content; you only need to modify the prompt to specify the course topic. In this example, we have used photography. The process includes creating a course outline, expanding each point of the outline into complete chapters, and further extending these chapters into more detailed sections. Below are the steps and components of the script.

## Requirements

- Python 3.7 or higher
- `.env` file with the OpenAI API key (`OPENAI_API_KEY`)
- Check the requirements file for the necessary dependencies.

## Installation

1. Clone this repository.
2. Install the required packages using `pip`:

    ```bash
    pip install -r requirements.txt
    ```

3. Create a `.env` file in the root directory of the project and add your OpenAI API key:

    ```
    OPENAI_API_KEY=your_api_key
    ```

## Usage

### Script Structure

The main script consists of the following sections:

1. **Loading Environment Variables:**

    ```python
    from dotenv import load_dotenv, find_dotenv
    load_dotenv(find_dotenv())
    os.environ['OPENAI_API_KEY'] = os.getenv("OPENAI_API_KEY")
    ```

    This section loads the environment variables from a `.env` file to configure the OpenAI API key.

2. **Function to Save Text to File:**

    ```python
    def save_to_file(filename, content):
        with open(filename, 'w', encoding='utf-8') as file:
            file.write(content)
    ```

    This function saves the generated content to text files.

3. **Language Model Initialization:**

    ```python
    from langchain.llms import OpenAI
    from langchain_openai import ChatOpenAI
    llm = ChatOpenAI(model="gpt-4")
    ```

    This section initializes OpenAI's GPT-4 model to generate the course content.

4. **Course Outline Generation:**

    ```python
    def generate_course_outline():
        outline_prompt = "Act as a photographer specializing in developing training content and generate an outline for a photography course aimed at small businesses wanting to learn how to take good photographs themselves in a DIY format. The outline should include at least 16 points."
        messages = [{"role": "user", "content": outline_prompt}]
        response = llm.invoke(messages)
        return response.content.strip()
    ```

    This function generates the course outline using a specific prompt.

5. **Expanding Outline Points into Chapters:**

    ```python
    def expand_point(point):
        expansion_prompt = f"Expand the following point from the outline into a complete chapter for the DIY photography course: {point}"
        messages = [{"role": "user", "content": expansion_prompt}]
        response = llm.invoke(messages)
        return response.content.strip()
    ```

    This function expands each point of the outline into a complete chapter.

6. **Extending Chapters into Detailed Sections:**

    ```python
    def expand_chapter(chapter):
        section_prompt = f"Further expand the following chapter of the DIY photography course: {chapter}"
        messages = [{"role": "user", "content": section_prompt}]
        response = llm.invoke(messages)
        return response.content.strip()
    ```

    This function extends each chapter into more detailed sections.

### Script Execution

1. **Generate the Outline:**

    ```python
    outline = generate_course_outline()
    save_to_file("course_outline.txt", outline)
    ```

    Generates and saves the course outline in a `course_outline.txt` file.

2. **Expand Outline Points into Chapters:**

    ```python
    outline_points = outline.split("\n")
    chapters = []
    for point in outline_points:
        chapter = expand_point(point)
        chapters.append(chapter)
        save_to_file(f"chapter_{outline_points.index(point) + 1}.txt", chapter)
    ```

    Expands each outline point into chapters and saves each chapter in separate files.

3. **Extend Chapters into Detailed Sections:**

    ```python
    complete_training_material = ""
    for chapter in chapters:
        section = expand_chapter(chapter)
        complete_training_material += section + "\n\n"
    save_to_file("complete_training_material.txt", complete_training_material)
    ```

    Extends each chapter into more detailed sections and saves all the training material in a `complete_training_material.txt` file.

### Output

The script generates and saves the following files:

- `course_outline.txt`: Contains the course outline.
- `chapter_<n>.txt`: Separate files for each chapter of the course.
- `complete_training_material.txt`: Contains all the complete and detailed training material.

### Final Note

This script automates the creation of educational material using the text generation capabilities of OpenAI's GPT-4 model, facilitating the production of high-quality educational content.


########################



# Generador de Material de Formación

Este repositorio contiene un conjunto de scripts en Python diseñados para generar material de formación. Utiliza el modelo de lenguaje GPT-4 de OpenAI para generar el contenido del curso que desees, solo tienes que modificar el prompt para indicarlo. En este caso en el ejemplo lo hemos hecho de fotografía. El proceso incluye la creación de un índice del curso, la expansión de cada punto del índice en capítulos completos, y la extensión de estos capítulos en secciones más detalladas. A continuación se detallan los pasos y componentes del script.

## Requisitos

- Python 3.7 o superior
- Archivo `.env` con la clave de API de OpenAI (`OPENAI_API_KEY`)
- Revisa el fichero requirements que contiene las dependencias que necesitas.

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
