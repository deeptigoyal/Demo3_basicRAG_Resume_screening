# Demo3_basicRAG_Resume_screening

**Intially About Agents:**

_Reflection, Tool, Planning, ReAct, and ReWOO_ are common agent patterns that guide how an AI reasons and takes actions. They are not full cognitive architectures, but each loosely aligns with parts of a modular system like perception, cognition, action, and learning.

- ReAct combines reasoning (“think”) and acting (“do”) step by step. The agent observes the input (perception), reasons about what to do next (cognition), and performs tool actions (action).
- Tool pattern focuses mainly on using external tools. The agent identifies the right tool and executes it. This is strongest on the “action” side.
- Planning pattern separates planning from execution. First the agent generates a detailed plan (cognition), then uses tools to execute it.
- Reflection pattern adds a feedback loop. After producing an answer or taking an action, the agent re-evaluates its own output, learns from mistakes, and improves—similar to a lightweight “learning” module.
- ReWOO (Reasoning WithOut Observation) separates reasoning from tool-use. The agent produces a reasoning trace without interleaving tool calls, ensuring consistency. Tools are used only after reasoning is complete. This emphasizes structured cognition before action.

All these patterns are modular, but they do not formally implement full perception–cognition–action–learning architectures; they simply borrow the structure to make reasoning cleaner and more reliable.

--------------------------------------
**ABOUT USECASE:**
--------------------------------------

Resume Screening Application with LangChain

Overview
This is a Resume Screening Application developed using LangChain that assists HR professionals in efficiently screening candidate resumes against job requirements.The application pulls text from uploaded resumes, analyzes them based on an AI model, and gives a detailed compatibility report with match scores, skills assessment, and suggestions.

Scenario
A company is looking for a Senior Python Developer post and receives hundreds of résumé applications. The HR team needs to rapidly filter these resumes to discover candidates whose abilities, experience, and qualifications best match the job criteria. Manually examining each CV is time-consuming and prone to human bias, leading to potential oversight of eligible individuals. The team requires an automated system to analyze applications against the job description, provide a complete compatibility assessment, and select the best prospects for further examination.

Problem Statement
Create a resume Screening Application using LangChain that lets HR managers enter job requirements, submit a candidate's résumé (in PDF, DOCX, or TXT format), and examine the resume to find fit for the post. The application should extract text from the resume, compare it against the job requirements using an artificial intelligence model, and offer a structured analysis including a match score, skills assessment, experience relevance, education evaluation, strengths, weaknesses, and general candidate recommendation. The answer has to be easy to use, handle several file types, and let users download the analysis report for keeping records.

Approach:-
Building a Resume Screening Application using LangChain and Streamlit, where users can:
- Input job requirements in a text area.
- Upload a resume in PDF, DOCX, or TXT format.
- Analyze the resume using Google’s Gemini 2.0 Flash model to get a detailed compatibility report.
- Download the analysis as a TXT file for record-keeping.

The application uses LangChain document loaders (PyPDFLoader, Docx2txtLoader, TextLoader) to extract text from resumes, and LangChain’s LLMChain with a custom prompt to generate a structured analysis. Streamlit provides a user-friendly interface with custom styling for better readability.

Project Structure
- app.py: Main application code for the Resume Screening Application.
- requirements.txt: List of dependencies required to run the application.
- .env: Environment file to store the Google API key (GOOGLE_API_KEY).
- venv/: Virtual environment directory (e.g., env for storing installed packages).

Setup Instructions:-
Create a Virtual Environment:
- Navigate to the project directory.
- Run: python -m venv venv (or env if preferred).
- Activate the virtual environment:
- Windows: venv\Scripts\activate
- macOS/Linux: source venv/bin/activate

Install Dependencies:
- Ensure requirements.txt is in the project directory.
- Run: pip install -r requirements.txt

Set Up the API Key:
- Create a .env file in the project directory.
- Add your Google API key: GOOGLE_API_KEY=your_api_key_here
- Ensure the .env file is loaded using python-dotenv (already included in app.py).

Run the Application Locally:
- Run: streamlit run app.py
- Open the provided URL (e.g., http://localhost:8501) in your browser to access the app.

Deploying on Streamlit Cloud:-

Prepare Your Project:
- Ensure app.py, requirements.txt, and .env are in the project directory.
- Create a Streamlit Cloud account at https://streamlit.io/cloud.

Upload to Streamlit Cloud:
- Log in to Streamlit Cloud.
- Create a new app and connect it to your project directory (e.g., upload the files manually or link to a cloud storage service).
- Specify app.py as the main script.

Configure Environment Variables:
- In Streamlit Cloud, go to your app’s settings.
- Add the GOOGLE_API_KEY as a secret environment variable (do not include .env in the uploaded files for security).

Deploy the App:
- vClick “Deploy” in Streamlit Cloud.
- Once deployed, access the app via the provided URL (e.g., https://your-app-name.streamlit.app).

Test the Deployed App:
- Input job requirements, upload a resume, and analyze it.
- Verify that the analysis report is generated and downloadable.

Requirements
The requirements.txt file includes all necessary dependencies.

Usage
- Run the app locally or access the deployed version on Streamlit Cloud.
- Enter job requirements in the text area (e.g., skills, experience, qualifications).
- Upload a resume in PDF, DOCX, or TXT format.
- Click “Analyze Resume” to generate the AI-driven analysis.
- Review the structured report, including match score, skills assessment, and recommendation.
- Download the analysis as a TXT file for records.
## 🚀 Features

- Upload resumes in PDF, DOCX, or TXT formats
- Extract and analyze resume content using Google Gemini model
- Score resume suitability in percentage
- Store analysis results in a Chroma vector store
- View structured AI-generated feedback
- Download analysis as a text report


-------------------------------------------------------
**Which patterns your code actually uses**
-------------------------------------------------------

A. TOOL PATTERN — YES (Strongly present)

What it means:
Tools = external capabilities (file loaders, embeddings, vector DB) that the LLM calls indirectly.

Where in your code:

PyPDFLoader, Docx2txtLoader, TextLoader → document extraction tools

GoogleGenerativeAIEmbeddings → embedding tool

Chroma → vector store tool

RunnableLambda inside RunnableMap → mini-tools to preprocess inputs

➡️ Your LLM is NOT doing everything. It relies on tools to extract text, embed, store, and search.
This is a classic TOOL pattern implementation.

B. PLANNING PATTERN — PARTIALLY (Weak)

Why only partial:
Planning = the agent or pipeline decides multi-step reasoning and executes steps in sequence.

Where in your code:
Not an agent.
You manually control the sequence:

Load → 2. Extract → 3. Split → 4. Analyze → 5. Parse score → 6. Store.

LLM is NOT planning;
your code is the plan.

➡️ So manual planning, not “planner-agent” style.

C. REACT PATTERN — NO

🔴 Your code does NOT implement ReAct (Reasoning + Acting)

Why ReAct does NOT apply:

ReAct requires LLM to:
Think → decide → call tool → observe → think → respond

Here the LLM does NOT:

decide which tool to use

see tool outputs as "observations"

do iterative loops

The LLM only receives a fixed prompt and returns one shot output.

➡️ No chain-of-thought reasoning + tool action loop → No ReAct.

D. REWOO PATTERN — NO

(REWrite, Execute, Write, Organize, Optimize)

Why REWOO does NOT apply:

REWOO is multi-agent, multi-document reasoning workflow

Used for research, aggregation from multiple sources

Needs rewriting tasks → executing tools → organizing evidence

Your pipeline is single query → single LLM call.
It does not:

rewrite plan

perform tool-calls mid-workflow

merge results

optimize tasks

➡️ Therefore REWOO is not applicable.

E. REFLECTION PATTERN — NO

Reflection means LLM:

generates a first answer

critiques it

refines it

produces improved answer

Your code:

calls LLM once

no improvement loop

no critique step

no self-consistency check

➡️ No reflection.

2️⃣ How  architecture maps to modular agent architecture


| Module                   | Present?                   | Where                                          |
| ------------------------ | -------------------------- | ---------------------------------------------- |
| **Perception Module**    | ✔️ YES                     | File loaders + extract_text_from_resume()      |
| **Cognitive Module**     | ✔️ YES                     | LLM analysis via PromptTemplate + Gemini       |
| **Action Module**        | ✔️ YES (but deterministic) | Vectorstore.add_documents(), formatting output |
| **Learning Module**      | ❌ NO                       | No model updating or RL loops                  |
| **Collaboration Module** | ❌ NO                       | No multi-agent behavior                        |



Why "learning" is NO

Vector DB ≠ learning

Storing chunks is memory, not training

Your LLM does not modify parameters

Why "collaboration" is NO

Only one LLM

No agent-to-agent communication

3️⃣ Summarized Explanation in 7 Lines

Your code uses TOOL pattern to extract text, embed, and store data.
It uses manual planning, not AI planning.
It does NOT use ReAct because LLM does not choose tools.
It does NOT use REWOO because no rewriting/execution/organization loop exists.
It does NOT use reflection because no iterative refinement exists.
Your architecture aligns with perception → cognition → action, but lacks learning and collaboration.
The system is a deterministic LCEL pipeline, not a multi-agent reasoning system


---
## 📦 Tech Stack

- **LangChain** for building chains, embeddings, and document processing
- **LangChain Expression Language (LCEL)** for modular pipeline workflows
- **Streamlit** for the frontend web interface
- **Google Generative AI** (Gemini & Embeddings) for LLM and vector representations
- **Chroma** as a persistent vector store
- **dotenv** for API key and environment config

---

## 🛠️ Setup Instructions
````

1. **Create and activate a virtual environment**

   ```bash
   python -m venv venv
   source venv/bin/activate   # or venv\Scripts\activate on Windows
   ```

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Add your API key**
   Create a `.env` file in the project root and add:

   ```
   GOOGLE_API_KEY=your_google_api_key
   ```

4. **Run the app**

   ```bash
   streamlit run app.py
   ```

---

## 📄 File Structure

```plaintext
├── resume.py                  # Main Streamlit app
├── chroma_store/           # Folder to store vector DB files
├── .env                    # Contains API key (not committed)
├── requirements.txt        # Python dependencies
└── README.md               # This file
```

---

## 📚 LangChain Concepts Used

* ✅ **Components & Modules**: PromptTemplate, LLM, Output Parsers
* 📄 **Document Loaders**: PDF, DOCX, TXT via LangChain community
* ✂️ **Text Splitting**: RecursiveCharacterTextSplitter
* 🧠 **Embeddings**: GoogleGenerativeAIEmbeddings
* 🗃️ **Vector DB**: Chroma for persistent storage
* 🧩 **LCEL**: RunnableMap, pipes (`|`), and chain composition
* 🧪 **Chains**: Custom chain for job/resume comparison
* 📤 **Deployment**: Streamlit as the UI layer

---

## 📈 Example Output

```
Structured Analysis:
- Strengths: Relevant experience, strong communication, etc.
- Weaknesses: Lacks X, missing Y...

Suitability Score: 84%
```

---

## 🧑‍💼 Ideal For

* HR professionals and recruiters
* Resume screening automation tools
* Educational and project demos for LangChain and LCEL


