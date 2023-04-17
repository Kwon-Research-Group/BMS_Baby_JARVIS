# The Baby JARVIS - BMS Ver. Prototype

<div align="center">
<br> <br>
 <b> "Good BMS, boss." </b>
<br> <br> <br>
<b> IMPORTANT: NOT GENERAL PURPOSE - Note that the keywords in the task queue are optimized for BMS tasks. </b> <br>  
Task queue의 키워드 구성 방식이 배터리 과제 수행을 위해 최적화되어 있습니다.
</div>
<p align="center"> <img width="50%" src="https://user-images.githubusercontent.com/86072294/232321509-52444906-53d8-40bd-bfbf-db5fd13c5016.jpg"/>

This is a task automation and execution device using OpenAI ChatGPT-4. Baby JARVIS aggregates the overall results and presents both specific and action plans for a single command. 

Three Big GPT command systems cycles for list-up: 1. Task Prioritization Agent (GPT-4) 2. Execution Agent (GPT-4) 3. Task Creation Agent (GPT-4)
Two tasking memories managed for finalization: 1. Task Queue 2. Memory

Developer: Byeongho Hwang (CrovaS, KAIST KRG), Gahyun Kim (CrovaG, Yonsei Univ CE), Jaehyun Lee (CrovaY, Seoul National Univ. CE), Woohyun Han (CrovaP, Google), Yohei Nakajima (Untapped Capital)

# System Diagram
Diagram is as follows:

<p align="center"> <img width="50%" src="https://user-images.githubusercontent.com/86072294/232323081-38d01835-1bf5-48c9-b559-e2641326224d.jpg"/>

1: Provide objective & tasks / 2: Complete task / 3: Send task result  
4: Add new tasks / 5: Prioritize tasks / 6: Cleaned task list  
A: (1) Store task / result pair (2) Query Memory for context / B: Query Memory for context / C: Context  

# Objective

This Python script is an example of an AI-powered task management system. The system uses OpenAI and Pinecone APIs to create, prioritize, and execute tasks. The main idea behind this system is that it creates tasks based on the result of previous tasks and a predefined objective. The script then uses OpenAI's natural language processing (NLP) capabilities to create new tasks based on the objective, and Pinecone to store and retrieve task results for context. This is a pared-down version of the original [Task-Driven Autonomous Agent](https://twitter.com/yoheinakajima/status/1640934493489070080?s=20) (Mar 28, 2023).

This README will cover the following:

- [How the script works](#how-it-works)

- [How to use the script](#how-to-use)

- [Supported Models](#supported-models)

- [Warning about running the script continuously](#continous-script-warning)

# How It Works<a name="how-it-works"></a>

The script works by running an infinite loop that does the following steps:

1. Pulls the first task from the task list.
2. Sends the task to the execution agent, which uses OpenAI's API to complete the task based on the context.
3. Enriches the result and stores it in Pinecone.
4. Creates new tasks and reprioritizes the task list based on the objective and the result of the previous task.
   </br>

The execution_agent() function is where the OpenAI API is used. It takes two parameters: the objective and the task. It then sends a prompt to OpenAI's API, which returns the result of the task. The prompt consists of a description of the AI system's task, the objective, and the task itself. The result is then returned as a string.
</br>
The task_creation_agent() function is where OpenAI's API is used to create new tasks based on the objective and the result of the previous task. The function takes four parameters: the objective, the result of the previous task, the task description, and the current task list. It then sends a prompt to OpenAI's API, which returns a list of new tasks as strings. The function then returns the new tasks as a list of dictionaries, where each dictionary contains the name of the task.
</br>
The prioritization_agent() function is where OpenAI's API is used to reprioritize the task list. The function takes one parameter, the ID of the current task. It sends a prompt to OpenAI's API, which returns the reprioritized task list as a numbered list.

Finally, the script uses Pinecone to store and retrieve task results for context. The script creates a Pinecone index based on the table name specified in the YOUR_TABLE_NAME variable. Pinecone is then used to store the results of the task in the index, along with the task name and any additional metadata.

# How to Use<a name="how-to-use"></a>

To use the script, you will need to follow these steps:

1. Clone the repository via `git clone https://github.com/Kwon-Research-Group/BMS_Baby_JARVIS.git` and `cd` into the cloned repository.
2. Install the required packages: `pip install -r requirements.txt`
3. Copy the .env.example file to .env: `cp .env.example .env`. This is where you will set the following variables.
4. Set your OpenAI and Pinecone API keys in the OPENAI_API_KEY, OPENAPI_API_MODEL, and PINECONE_API_KEY variables.
5. Set the Pinecone environment in the PINECONE_ENVIRONMENT variable.
6. Set the name of the table where the task results will be stored in the TABLE_NAME variable.
7. (Optional) Set the objective of the task management system in the OBJECTIVE variable.
8. (Optional) Set the first task of the system in the INITIAL_TASK variable.
9. Run the script.

All optional values above can also be specified on the command line.

# Running inside a docker container

As a prerequisite, you will need docker and docker-compose installed. Docker desktop is the simplest option https://www.docker.com/products/docker-desktop/

To run the system inside a docker container, setup your .env file as per steps above and then run the following:

```
docker-compose up
```

# Supported Models<a name="supported-models"></a>

This script works with all OpenAI models, as well as Llama through Llama.cpp. Default model is **gpt-3.5-turbo**. To use a different model, specify it through OPENAI_API_MODEL or use the command line.

## Llama

Download the latest version of [Llama.cpp](https://github.com/ggerganov/llama.cpp) and follow instructions to make it. You will also need the Llama model weights.

- **Under no circumstances share IPFS, magnet links, or any other links to model downloads anywhere in this repository, including in issues, discussions or pull requests. They will be immediately deleted.**

After that link `llama/main` to llama.cpp/main and `models` to the folder where you have the Llama model weights. Then run the script with `OPENAI_API_MODEL=llama` or `-l` argument.

# Warning<a name="continous-script-warning"></a>

This script is designed to be run continuously as part of a task management system. Running this script continuously can result in high API usage, so please use it responsibly. Additionally, the script requires the OpenAI and Pinecone APIs to be set up correctly, so make sure you have set up the APIs before running the script.
