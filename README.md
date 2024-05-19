# CREWAI RAG LANGCHAIN QDRANT 🤼

<p align="center">
  <img width="976" alt="crewai" src="https://github.com/benitomartin/benitomartin.github.io/assets/116911431/a1fe4ed5-1e04-4a02-94ee-8514cb9bfd40">
</p>

This repository contains a full Q&A pipeline using LangChain framework, Qdrant as vector database and CrewAI as Agents. The data used is "The Attention Mechanism" research paper, but the RAG pipeline is structure to analyze research papers and provide an analysis and summary. Therefore it can use any other research paper. Use this **[Link](https://nbviewer.org/github/benitomartin/crewai-rag-langchain-qdrant/blob/main/crewAI_RAG_Qdrant.ipynb)** if the notebook cannot be opened.

The main steps taken to build the RAG pipeline can be summarize as follows:

* **Data Ingestion**: load data from pdf file

* **Indexing**: RecursiveCharacterTextSplitter for indexing in chunks

* **Vector Store**: Qdrant inserting metadata

* **QA Chain Retrieval**: RetrievalQA

* **CrewAI**: Researcher and Writer Agents to analyze and summarize the paper
  
Feel free to ⭐ and clone this repo 😉

## 👨‍💻 **Tech Stack**


![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![OpenAI](https://img.shields.io/badge/OpenAI-74aa9c?style=for-the-badge&logo=openai&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white)


## 📐 Set Up

In the initial project phase, the documents are loaded using **PyPDFLoader** and indexed. Indexing is a fundamental process for storing and organizing data from diverse sources into a vector store, a structure essential for efficient storage and retrieval. This process involves the following steps:

- Select a splitting method and its hyperparameters: we will use the **RecursiveCharacterTextSplitter**.

- Select the embeddings model: in our case the **OpenAI**

- Select a Vector Store: **Qdrant**.

Storing text chunks along with their corresponding embedding representations, capturing the semantic meaning of the text. These embeddings facilitate easy retrieval of chunks based on their semantic similarity. 

After indexing, a QA Chain Retrieval Pipeline is set up in order to check the Q&A functioning and performance. 

The final step includes CrewAI Agents to generate two documents:

- An analysis of the paper provided by the research agent

- A summary of the key findings provided by the writer


## 🌊 QA Chain Retrieval Pipeline

The pipeline created contains the main llm model, the QA chain and the agents. A custom tool which uses the vector store as search engine has been implemented. The researcher looks for the paper and generates a detailed [analysis](https://github.com/benitomartin/crewai-rag-langchain-qdrant/blob/main/researcher_analysis.md) as md file. Finally the writer generates a short [summary](https://github.com/benitomartin/crewai-rag-langchain-qdrant/blob/main/writer_summary.md) with the key findings of the paper, trying to write it in a way that it is comprehesive for computer science students.

```
### RESEARCHER ###

# Agent
reasarcher = Agent(
    role="Computer Science Researcher",
    goal='Understanding the published research paper and extract the main insights',
    backstory="""You work at a Computer Science school.
                  A new paper has been published and you need to extract
                  the main information and provide the main insights to
                  the students.You have a knack for dissecting complex
                  research papers""",
    verbose=True,
    tools=[tool_instance],
    allow_delegation=False
    )


# Tasks
task_research = Task(
    description="""Conduct an in-depth analysis of the published research paper.
                    Identify the key components and try to understand how each works.""",
    expected_output="""Your final answer must be a full analysis report of the research paper""",
    agent=reasarcher,
    output_file='researcher_analysis.md' 

)
```

```
### WRITER ###

# Agent
writer = Agent(
    role="Computer Science Writer",
    goal= 'Craft easy to understand content for Computer Science students',
    backstory="""You are working on writing a summary of a research paper.
                  Use the content of the Computer Science Researcher to develop
                  a short comprehensive summary of the research paper""",
    verbose=True,
    allow_delegation=False,
)

# Tasks
task_writer = Task(
    description="""Extract the most important topics and key components of the report
                    developed by the Computer Science Researcher.""",
    expected_output="""Your final answer must be a short comprehensive summary 
                        to understand report for Computer Science students""",
    agent=writer,
    output_file='writer_summary.md' 

)
```
