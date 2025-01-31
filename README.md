# Arthantar

## Contextual Translation Integrating Coreference Analysis, Large Language Models, Reinforcement Learning and Knowledge Graphs

Arthantar is a sophisticated multi-layered system designed to enhance translation accuracy by integrating gender identification, coreference resolution, and knowledge graph (KG) generation. This approach ensures accurate and contextually relevant translations by leveraging cutting-edge technologies and fallback mechanisms.

### Key Features

A sophisticated multi-layered approach to improve translation by utilizing a combination of gender identification, coreference resolution, and knowledge graph (KG) generation. The core of this approach relies on multiple technologies and models, each serving a specific function. For gender identification, I used the FastCoref module, a coreference resolution tool that identifies gendered pronouns and assigns genders to entities based on context. The module works by analyzing the text and identifying clusters of related pronouns, using predefined sets of male and female pronouns to determine the likely gender of each entity. I tested four different coreference models and selected the one that best resolved ambiguous pronouns and determined gender accurately, with a focus on proper noun gender inference.

To further enhance the translation context, I incorporated an advanced large language model (LLM) using the Groq API. For gender identification when coreference resolution fails, I used the mixtral-8x7b-32768 model, which is employed as a backup to predict the likely gender of entities, specifically persons, based on contextual clues such as the name or role described. If the coreference model doesn’t provide a clear gender, the system sends a prompt to the LLM, asking it to analyze the entity name and its type (e.g., a person) and predict its gender. This dual-layered approach ensures that gender is identified accurately, even in cases where coreference models face difficulties, such as when pronouns are not explicitly mentioned.

For the knowledge graph generation, I used the LLMGraphTransformer library, which facilitates the creation of a knowledge graph that encapsulates entities and their relationships. The knowledge graph is fed into the mixtral-8x7b-32768 model, which processes the input text and extracts the relevant information to generate graph documents. This graph not only includes the entities but also incorporates gender data and relationships between them. The knowledge graph is built by first converting the input text into graph documents, then creating nodes and relationships using the transformer’s output. These nodes are enriched with the gender information identified by the coreference resolution or LLM. The knowledge graph plays a crucial role in providing contextual information to the translation process, ensuring that the translation model can handle gender nuances and understand the relationships between entities, leading to a more accurate translation.

In case both the coreference and LLM fail to provide sufficient data, a more sophisticated backup method is employed using spaCy's natural language processing capabilities. This backup knowledge graph is built using spaCy's named entity recognition (NER) and dependency parsing features. The system extracts named entities (such as persons, organizations, and locations) and their relationships through syntactic dependency analysis. Using spaCy's en_core_web_sm model, the system identifies entity types, tracks their mentions throughout the text, and establishes meaningful relationships based on sentence structure. The NetworkX library is used to create a directed graph where entities are connected based on their syntactic relationships rather than just linear sequence. If the graph documents from the LLMGraphTransformer are empty or the coreference fails to resolve entities, this spaCy-enhanced backup structure is used. The system implements multi-level fallback through exception handling: if the coreference model encounters errors, it defaults to the LLM; if the LLM fails, it uses the spaCy-based graph generation; and if spaCy processing fails, it falls back to a basic entity extraction using capitalized words. This ensures that, regardless of the failure point, a semantically meaningful graph is always available to provide contextual information for the translation model, with each fallback level maintaining decreasing but still useful levels of sophistication. The LLaMA-3.1-8b is fine-tuned using DeepSpeed.

### Workflow

![Workflow Diagram](images/workflow_arthantar.png)

### Technologies Used

- **Coreference Resolution**: FCoref module and mixtral-8x7b-32768 as backup.
- **Large Language Models**: mixtral-8x7b-32768 model for knowledge graph and gender backup; Llama-3.1-8b for translation.
- **Knowledge Graph**: LLMGraphTransformer library.
- **Dependency Graphs**: NetworkX, SpaCy’s en_core_web_sm.
- **Error Handling**: Multi-level fallback hierarchy ensuring robust output generation.
- **Llama Finetuning**: DeepSpeed.


