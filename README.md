# Knowledge Graph-based Question and Answer System

This project is an implementastion of a Knowledge Graph-based Question and Answer System.
The knowledge graph used in this project is [DBpedia](https://www.dbpedia.org). 

---
The system is composed of 3 parts:
1. Translating the question in natural language into a SPARQL query
2. Extracting information from the DBpedia
3. Generating an answer in natrual language based on the extracted facts
---
Created system is capable of answering simple questions (that is those that can be represented by a single triplet of the form <Subject - Predicate - Object>). 
The way the translation works is by analyzing the question's 3 main components: 
1. The entity (The subject or the object of the question)
2. The predicate (The required property of the subject/object)
3. Direction of the question (Forward/Backward, depending on what is missing from the triplet, either Object or Subject)

* __Named Entity Recognition__. Done with SPACy's pretrained model followed by DBpedia Spotlight api to link extracted entities to the acual DBpedia resources.
* __Predicate Classification__. Done with huggingface pretrained DistilBERT model that was then fine-tuned on task-specific training data. Each question gets assigned one of the possible predicates, that is properties, that an entity can have.
* __Question Direction Classification__. Done with a simple GRU model, a simple Binary Classification task.
---

Queries are formed via templates that are filled with extracted data and are then passed to DBpedia Virtuoso SPARQL API.

The system's chatbot works as a Finite State Machine, extracting information from the question one step at a time and then forming a response. The response is generated using ollama's Mistral LLM. 

Developed system still fails to answer certain question, mostly due to extracted predicates being unapplicable to extracted entities (no relevant information found in DBpedia).

---
_Possible ways of improving the system is:_
- Adding support for more question types
- Finding more data to train the models on
- Adding more nuanced question analysis (support for constrained types of answer)

___

# Система вопросов и ответов на основе графа знаний

Этот проект представляет собой реализацию системы вопросов и ответов на основе графа знаний.  
В качестве графа знаний в проекте используется [DBpedia](https://www.dbpedia.org).  

---  
Система состоит из 3 частей:  
1. Перевод вопроса на естественном языке в SPARQL-запрос  
2. Извлечение информации из DBpedia  
3. Формирование ответа на естественном языке на основе извлечённых фактов  
---  

Созданная система способна отвечать на простые вопросы (те, которые могут быть представлены единичным триплетом вида <Субъект - Предикат - Объект>).  
Перевод вопроса работает за счёт анализа 3 основных компонентов:  
1. Сущность (Субъект или объект вопроса)  
2. Предикат (Искомое свойство субъекта/объекта)  
3. Направление вопроса (Прямое/Обратное, в зависимости от того, какой элемент триплета отсутствует - Объект или Субъект)  

* __Распознавание именованных сущностей (NER)__. Выполняется с помощью предобученной модели SPACy с последующим использованием DBpedia Spotlight API для привязки извлечённых сущностей к ресурсам DBpedia.  
* __Классификация предикатов__. Используется предобученная модель DistilBERT от HuggingFace, дообученная на специфичных для задачи данных. Каждому вопросу присваивается один из возможных предикатов - свойств, которыми может обладать сущность.  
* __Классификация направления вопроса__. Реализовано простой GRU-моделью как задача бинарной классификации.  
---  

Запросы формируются по шаблонам, которые заполняются извлечёнными данными и передаются в Virtuoso SPARQL API DBpedia.  

Чат-бот системы работает как конечный автомат, извлекая информацию из вопроса шаг за шагом и затем формируя ответ. Ответ генерируется с помощью LLM Mistral от ollama.  

Разработанная система всё ещё ошибается при ответе на некоторые вопросы, в основном из-за того, что извлечённые предикаты неприменимы к найденным сущностям (в DBpedia отсутствует соответствующая информация).  

---  
_Возможные пути улучшения системы:_  
- Добавление поддержки большего количества типов вопросов  
- Поиск дополнительных данных для обучения моделей  
- Более детальный анализ вопросов (поддержка ограниченных типов ответов)


