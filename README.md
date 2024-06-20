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


