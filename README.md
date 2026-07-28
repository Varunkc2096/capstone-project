# capstone-project
This repository contains the complete submission for the Zepto Data & AI Platform Capstone Project. The project simulates an end-to-end AI/ML engineer workflow, moving from raw data extraction to predictive analytics and finally to a deployed Generative AI service.

The project is structured into three distinct modules, all contained within this single public repository.

## Repository Structure

*   **/data_pipeline:** Contains the data engineering pipeline. It demonstrates scraping live catalog data, cleaning and transforming the data, applying a fixed-rate currency conversion, and loading the final dataset into a normalized SQLite database.
*   **/analytics:** Contains the data science workflow. It covers exploratory data analysis (EDA), handling missing values and outliers on the Titanic dataset, followed by building, evaluating, and tuning a full predictive modeling pipeline (including Random Forest, Logistic Regression, and Decision Trees).
*   **/support_assistant:** Contains the GenAI support assistant. It uses a LangGraph-orchestrated flow to route queries and retrieve context from a local ChromaDB vector store containing Zepto policy documents, all wrapped in a locally runnable FastAPI application via Docker.

## Setup & Installation

Each module is designed to be self-contained. The dependencies for the entire project are combined here at the root.

**Requirements:**
*   Python 3.7+
*   Docker (for Module 3 containerization)

**Installation:**
1.  Clone this repository:
    ```bash
    git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
    cd YOUR_REPO_NAME
    ```
2.  Install all required Python packages:
    ```bash
    pip install -r requirements.txt
    ```

## Execution Guide

### Module 1: Data Pipeline (`/data_pipeline`)
1.  Navigate to the `/data_pipeline` directory.
2.  Open and run the `data_pipeline.ipynb` notebook sequentially to execute the web scraping, data cleaning, and database generation steps.
3.  The final output is a populated `books_project.db` SQLite database.
4.  *See `/data_pipeline/README.md` for specific design and cleaning decisions.*

### Module 2: Analytics (`/analytics`)
*(To be completed)*
1.  Navigate to the `/analytics` directory.
2.  Follow the instructions in the corresponding notebook(s) to view the EDA and modeling pipeline.

### Module 3: Support Assistant (`/support_assistant`)
*(To be completed)*
1.  Navigate to the `/support_assistant` directory.
2.  Instructions for building and running the FastAPI Docker container will be located here.

## General Design Decisions
*   **Version Control:** The repository follows a standard Git workflow, utilizing feature branches for module development before merging into the main branch to ensure a clean commit history.
*   **No Paid APIs:** All modules are designed to run using local compute or free-tier services, adhering to the project's strict requirement for a fully offline or cost-free implementation.

---
*This repository represents a single, coherent submission for the Capstone Project - Certificate Program in Artificial Intelligence and Machine Learning.*
