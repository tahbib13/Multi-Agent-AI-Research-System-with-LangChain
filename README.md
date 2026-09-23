Step 01 -> Environment Setup

Create a virtual environment , install al the libraries from requirements.txt and create .env file with your OpenAI, Tavily, Weather, MistralAi, Gemini Api keys.

Go to new terminal and type : uv venv

Then Activate the virtual environment : .venv\Scripts\activate

Then install requirements.txt file : uv pip install -r requirements.txt

.env ->

OPENAI_API_KEY = ""

GROQ_API_KEY = ""

GOOGLE_API_KEY = ""

MISTRAL_API_KEY = ""

HUGGINGFACEHUB_API_TOKEN=

TAVILY_API_KEY = ""

OPENWEATHER_API_KEY = ""

In my case I used Google Api Key.

Step 02 -> Create tool.py

We will build 2 custom tools using @tool decorator. First the web_search tool which talks to the Tavily API and fetches live search results from the internet. Second the scrape_url tool which takes a URL, visits that page and extracts clean readable text from it using BeautifulSoup.

Step 03 -> Create agents.py

This is the heart of the project. We will build 4 things here, First the Search Agent using create_agent +AgentExecutor which will use the web_search tool. Second the reader agent using the same pattern but with scrape_url tool. Third the writerchain using the modern LCEL pipe ssystax – prompt | llm | StrOutParser() which takes all the research and writes a full report. Fourth the Critic Chain again using LCEL pipe which reads the report and gives a score and feedback.


Step 04 -> Create pipeline.py

This is the supervisor. We will write one function called run_research_pipeline that calls all 4 agents and chains in the correct order and passes results between them using a shared  state dictionary. The agents use message based input/output, we send {“message”: [“user”, “…..”]} and read the response from result [“messages”][-1].content. At the end of each step it will print the output in the terminal so students can see exactly what each agent is doing.

Step 05 ->Run and Test

Run python pipeline.py in the terminal, enter a research topic and watch all 4 agents work one by one – search, read, write, review and print the final report with critic feedback right inn the terminnal.

Step 06 -> Development The pipeline into UI using streamlit

UI File is  app.py

Running command : streamlit run app.py

Diagram Section :

System Architecture Diagram

<img width="1408" height="768" alt="Image 1 System Architecture Diagram (Steps 01–05)" src="https://github.com/user-attachments/assets/2c898b16-bc9f-431b-89a9-bdf8d881a924" />

End-to-End Implementation Pipeline (Steps 01–05)

<img width="1408" height="768" alt="Image 2 End-to-End Implementation Pipeline (Steps 01–05)" src="https://github.com/user-attachments/assets/03d964ad-f8fa-49c1-964c-7570de988a2e" />



