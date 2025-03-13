# aws-bedrock-study-notes

Based on: https://github.com/Patotricks15/amazon-bedrock-workshop


## 01 - Text Generation

### 1. API and Request Basics
#### What API is used to send requests to foundation models in Amazon Bedrock?
- The InvokeModel API is used to send requests to foundation models in Amazon Bedrock.

#### Which Python SDK is used to interact with Amazon Bedrock?
- Boto3 is used to interact with Amazon Bedrock and send API requests.

#### How does Boto3 differ from LangChain in interacting with Amazon Bedrock?
- Boto3 provides direct control over request parameters, whereas LangChain simplifies API calls by wrapping Boto3 operations.

### 2. Key Parameters in API Requests
#### What does the maxTokenCount parameter control?
- It sets the maximum number of tokens in the generated response.

#### What is the purpose of the stopSequences parameter?
- It defines where the model should stop generating text, preventing unwanted continuations.

#### How does the temperature parameter affect the response?
- It controls the creativity of the response:

  - 0 = Minimal variability, useful for reproducibility.

  - 1 = High variability, useful for creative tasks.

#### What is the role of the topP parameter?
- It limits the selection of tokens based on probability, focusing on the most likely words when set to a lower value.


### 3. Writing Effective Prompts for Summarization
#### What is an important structure to follow when writing a text summarization prompt?
- The prompt should start with a clear instruction (e.g., “Summarize the following text”) and the text should be enclosed within <text> tags.

#### Why should the text length be within the model’s token limit?
- If the text exceeds the model’s token limit, it may get truncated, leading to incomplete summaries.

### 4. Applications and Benefits
#### What are some use cases of Amazon Bedrock’s text generation capabilities?
- Applications include:

  - Summarizing technical documents and research papers.

  - Generating reports and study notes.

  - Extracting knowledge from large text sources.

#### How can adjusting temperature and topP help in different applications?
- Below:
  - Low temperature & high topP = More structured and predictable responses (good for summarization).

  - High temperature & lower topP = More creative and diverse text (good for content generation).


### 1. Basics of API Requests
#### What is the purpose of using Boto3 with Amazon Bedrock?
- Boto3 allows users to send API requests to Amazon Bedrock to invoke foundation models for various tasks like text generation and summarization.

#### What are the three essential parameters required in an API request?
- modelId – Specifies the foundation model to use.
- accept – Defines the expected response format.
- contentType – Specifies the request body format.


### 2. Understanding Request Parameters
#### What does the modelId parameter do in an API request?
- It specifies which foundation model should be used for processing the request.

#### What value is typically used for the accept parameter?
- "application/json", which indicates that the response should be returned in JSON format.

#### What is the purpose of the contentType parameter?
- It defines the format of the request body, typically set to "application/json".

### 3. Functionality of Foundation Models
#### How does a foundation model process an API request?
- The model analyzes the input prompt and generates a response based on its pretrained knowledge and configuration parameters.

#### In the given example, what task is the foundation model performing?
- The foundation model is used to summarize a given text.

## 02 - Knowledge bases and RAG

### 1. Basics of OpenSearch Serverless
#### What is Amazon OpenSearch Serverless?
A1: It is a serverless option in Amazon OpenSearch Service that allows developers to run petabyte-scale workloads without managing OpenSearch clusters.

#### What are the key benefits of OpenSearch Serverless?
- Automatic scaling based on workload demand.
- Interactive millisecond response times.
- Pay-per-use pricing model.
- No need to manually configure or manage clusters.

### 2. Creating a Knowledge Base in OpenSearch Serverless
#### What are the key steps in creating a Knowledge Base (KB)?
- Initialize OpenSearch Serverless configuration (collection ARN, index name, vector field, text field, metadata field).
- Define chunking strategy to split documents based on chunk size.
- Configure S3 data source for document storage.
- Set up Titan embeddings model ARN to generate vector embeddings.

#### Why is chunking important in the Knowledge Base setup?
- Chunking splits documents into smaller pieces of a defined size, making it easier to process and store embeddings for efficient retrieval.

#### What role does the Titan embeddings model ARN play?
- It is used to convert text chunks into embeddings, which are then stored in the OpenSearch Serverless index for efficient retrieval.



### 3. Ingestion Process
#### What happens during the ingestion job?
- KB fetches documents from the data source.
- It extracts text and chunks it based on the chunking strategy.
- The Titan model creates embeddings for each chunk.
- The embeddings are stored in the vector database (OSS).

#### Why is OpenSearch Serverless useful for document ingestion?
- It automatically scales, efficiently processes large-scale text data, and optimizes searches for fast retrieval.

### 4. Using RetrieveAndGenerate API
#### What does the RetrieveAndGenerate API do?
- Converts user queries into embeddings.
- Searches the knowledge base for relevant context.
- Augments the foundation model prompt with retrieved information.
- Generates a contextual response based on the results.

#### What additional information does the RetrieveAndGenerate API return?
- The generated response.
- Source attribution of retrieved data.
- Retrieved text chunks for reference.

### 5. Retrieve API Functionality
#### How does the Retrieve API differ from RetrieveAndGenerate API?
- Retrieve API: Only fetches relevant text chunks and metadata for custom workflows.
- RetrieveAndGenerate API: Enhances queries, searches the KB, and returns an AI-generated response with contextual augmentation.

#### What information does the Retrieve API provide?
- Retrieved text chunks.
- Location type and URI of the source data.
- Relevance scores for search results.

### 1. Functionality of RetrieveAndGenerate API
#### What is the primary function of the RetrieveAndGenerate API?
- It converts queries into embeddings, searches the knowledge base, augments the foundation model prompt with retrieved context, and generates a response using a foundation model.

#### How does the RetrieveAndGenerate API improve responses?
- It retrieves relevant knowledge from a database and uses it as context to enhance the foundation model's response.

### 2. Multi-Turn Conversations and Context Retention
#### How does the API handle multi-turn conversations?
- It manages short-term memory of the conversation to provide more contextual and coherent responses across multiple interactions.

#### Why is short-term memory important in conversational AI?
- It allows the system to remember past exchanges, making responses more relevant and context-aware over time.

### 3. Output of RetrieveAndGenerate API
#### What are the key components of the RetrieveAndGenerate API output?
- Generated response from the foundation model.
- Source attribution (where the retrieved data came from).
- Retrieved text chunks used for context in the response.

#### How does source attribution benefit users?
- It provides transparency by showing where the model retrieved the supporting information from.

### 4. Building Model Invocation Chains
#### What is the role of create_stuff_documents_chain?
- It feeds the retrieved context into a prompt and LLM by "stuffing" all retrieved content into the prompt without summarization or processing.

#### How does create_retrieval_chain enhance the process?
- It adds a retrieval step, propagates the retrieved context through the chain, and outputs input, context, and answer together.

## 03 - Model customization

### 2. Fine-Tuning the Amazon Titan Text Model

#### What are the key hyperparameters for customizing the Amazon Titan model?
- epochs: Number of training iterations (1-10, default: 5).
- batchSize: Number of samples processed per update (1-64, default: 1).
- learningRate: Step size for updating model parameters (0.0-1.0, default: 1.00E-5).
- learningRateWarmupSteps: Steps over which learning rate increases (0-250, default: 5).

#### Why is fine-tuning useful?
- It helps adapt the model to specific use cases by improving response accuracy and relevance based on training data.

### 3. Evaluating Model Performance
#### What metric is used to evaluate fine-tuning performance?
- BERTScore, which measures similarity between a generated summary and reference text using BERT embeddings.

#### What is the purpose of BERTScore in evaluation?
- It compares the fine-tuned model’s output with a base model to check if fine-tuning improved text quality.

#### Why is BERTScore effective for evaluation?
- It captures semantic meaning similarity rather than relying on exact word matching, making it more robust.

## 05 - Agents

### 1. Understanding Inline Agents
#### What are Inline Agents in Amazon Bedrock?
A1: Inline Agents are dynamically configurable AI assistants that can be modified in real time to adjust their behavior, capabilities, and knowledge base.

#### How do Inline Agents differ from traditional static agents?
A2: Unlike static agents, Inline Agents can be modified at runtime, allowing real-time adjustments to instructions, integrations, and responses.

#### What are the key features of Inline Agents?
- Dynamic configuration: Modify agent behavior on the fly.
- Flexible integration: Connect to external APIs and services.
- Contextual adaptation: Adjust responses based on user roles and scenarios.

### 2. Benefits of Using Inline Agents
#### Why are Inline Agents beneficial for AI applications?
- Rapid prototyping: Test different configurations without redeploying.
- Personalization: Customize responses and behavior in real time.
- Scalability: Manage a single agent across multiple functions.
- Cost-effectiveness: Optimize resource usage dynamically.

#### How do Inline Agents contribute to cost efficiency?
- They use only the necessary tools and knowledge per interaction, reducing computational costs.

### 3. Invoking an Inline Agent
#### How can an Inline Agent be invoked in Amazon Bedrock?
- By using the InvokeInlineAgent API via the boto3 bedrock-agent-runtime client.

#### What is the purpose of the invoke_inline_agent_helper function?
- Processes agent trace requests for better readability.
- Displays the code used by the interpreter.
- Shows function invocations and knowledge base content.

#### What additional metrics are provided when invoking an Inline Agent?
- Invocation time (how long the request took).
- Input and output tokens used during the interaction.

## 06 Ragas Agent Evaluation

### 1. Understanding Agent Goal Accuracy
#### What is Agent Goal Accuracy?
- It is a binary metric (0 or 1) that evaluates how well an LLM identifies and achieves user goals.

#### How is Agent Goal Accuracy evaluated?
- Using an evaluator LLM (e.g., Amazon Nova Pro) to assess whether the AI successfully accomplished the intended goal.

### 2. Agent Goal Accuracy Variants
#### What are the two types of Agent Goal Accuracy?
- AgentGoalAccuracyWithoutReference – No predefined reference; evaluates based on inferred success from conversation context.
- AgentGoalAccuracyWithReference – Compares AI’s response against a predefined reference outcome.
#### When is AgentGoalAccuracyWithoutReference useful?
- When explicit reference outcomes are unavailable, and the success criteria can be inferred from the conversation.

####What additional input does AgentGoalAccuracyWithReference require?
- user_input (the query or task).
- reference outcome (the expected ideal result).
#### What tool is used to encapsulate multi-turn conversations for evaluation?
- The MultiTurnSample object in the ragas ecosystem.

### 3. Agent Goal Accuracy Results & Observations
#### What patterns were observed in different personas' evaluations?
- Andrew’s conversations → High accuracy (1.0) due to focused, goal-oriented approach.
- Jane’s conversations → Low accuracy (0.0) due to lack of historical information. The system fails to provide suggestions in a single conversation turn.
#### How can Jane’s results be improved?
- A human-in-the-loop approach asking for her interests could help provide better suggestions.

#### How can results for AgentGoalAccuracyWithReference be influenced?
- By adjusting either the conversation flow or the reference outcome, allowing greater flexibility in evaluation.

### 4. Understanding Tool Call Accuracy
#### What is Tool Call Accuracy?
- A metric that evaluates how accurately the LLM identifies and invokes necessary tools to complete a task.

#### What inputs are needed to calculate Tool Call Accuracy?
- user_input (task or query).
- reference_tool_calls (expected tool invocations).
#### How is Tool Call Accuracy computed?
- By comparing actual tool calls made by the AI against the annotated reference tool calls.

#### Why is an evaluator LLM not required for Tool Call Accuracy?
- Because it uses direct comparison between reference and actual tool calls instead of semantic evaluation.

### 5. Tool Call Accuracy Evaluation Process
#### How are conversations structured for evaluation?
- Using the MultiTurnSample object in ragas to handle multi-turn conversations.

#### What method is used to compare tool call arguments?
- The NonLLMStringSimilarity metric, which leverages string distance measures like:
  - Levenshtein
  - Hamming
  - Jaro
#### Why is NonLLMStringSimilarity useful for this evaluation?
- Because it compares tool arguments effectively without relying on an LLM for semantic understanding.
