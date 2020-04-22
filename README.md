# Tasks via Dialogue in Different Service Scenarios
## Ruolin Su 3/31/2020
There is a large amount of task-oriented dialogue corpus created to solve specific tasks in different domains and scenarios. Here I collected some of them, which may cover most popular benchmarks for research. Such that we compare and utilize dialogue corpus to complete feasible tasks.

### Task 1 The Delivery/Purchase Order Automation Task 

The ultimate goal for delivery/purchase order automation via dialogue is to generate a structured order (i.e. JSON format) by mining from dialogue context and assigning semantic roles to different entities mentioned in the dialogue. Based on dialogue history and current action, the system is required to fill and update slots within the order frame. Which means, name entity recognition, especially on address and time, and slot filling are of importance in delivery order automation task. 

<p align="center">
  <img src="https://github.com/youlandasu/service_dialogue/blob/master/summarization.png">
</p>

*An example of order summarization from dialogue*

One of the main challenges of order automation is to design a mechanism to ensure the logic, integrity, and correctness of the structured output. This is quite similar to the dialogue summarization problem, in which each utterance is associated with a speaker. Before neural networks were introduced to summarization, feature engineering and template selection was adopted to summarize the dialogues, which requires a large manual effort and limits its portability. Other work introduces the unsupervised learning for summarization in the scenarios where labeled data lacks. Then neural based methods are applied for this task, including a hierarchical encoder to catch the interactions and using auxiliary dialogue act to assist the summarization, where each utterance is assigned a dialogue act to label its effect on the interactions. Liu et al. propose a hierarchical encoder to model dialogues using key point sequences as auxiliary labels to ensure the logic and integrity of the summary (2019). However, corpus related to this problem such as SAMSum (*The dataset will be published on ELRA language resources catalogue.*) corpus, [AMI meeting][12] corpus and [argumentative dialogue summary][11] corpus are non-oriented for chitchat, aiming to recognize whether central propositions realize the same facet of the argument in the dialogue. Didi’s customer service dialogs (Liu et al., 2019) are more in line with the delivery/purchase order automation task, but not publicly accessible right now.

### Task 2 The Automatic Diagnosis Task

As electronic health records (EHR) become popular in modern health system, automatic and remote medical also has a demand of diagnosing directly from EHR as an assistance to human effort. EHR consists of a piece of self-report and a conversation initialized by a doctor. A diagnosis is made based on both the self-report and the conversational data, extracting and normalizes symptoms (from both data) and attempts to make a diagnosis for the user to give a reasonable act. Thus, symptom entity extraction, entity normalization, context combination and entity matching are required in processing.

Wei et al. proposed dataset [Muzhi][5] is collected from the pediatric department in a Chinese online healthcare community (2018). In this setting, a patient would provide a self-report and then a conversation initialized by a doctor collect more information and make a diagnosis based on both the self-report and the conversational data. The user goal is to fill in slots related to symptoms and disease. 

<p align="center">
  <img src="https://github.com/youlandasu/service_dialogue/blob/master/diagnosis1.png">
</p>

*Example of user record*

Task-oriented dialogue system typically contains components of Natural Language Understanding (NLU), Dialogue Manager (DM) and Natural Language Generation (NLG). NLU detects the user intent and slots with values from utterances; and NLU follows follow the BIO (begin-in-out) schema and normalization methods for symptom identification. DM tracks the dialogue states and takes system actions, consisting of two submodules dialogue state tracker (DST) and policy learning. NLG generates natural language given the system actions, based on templates or learning from language models.

<p align="center">
  <img src="https://github.com/youlandasu/service_dialogue/blob/master/diagnosis2.png">
</p>

*Example of user goal, where ‘disease slot’ is given by inferring from extracted symptoms.*

### Task 3 The Conversational Movie Recommendation Task

In the movie domain, we expect a dialogue system with abilities to give movie-related facts to a user and perform insightful movie recommendation. Such that four sub-tasks are listed here:

*	Question Answering (QA): Focus on the ability to answer factoid questions that can be answered without relation to previous dialog. The context consists of the question only.

*	Recommendation: Focus on the ability to provide personalized responses to the user via recommendations of movies’ name rather than other related facts as above.

*	Joint QA and recommendation: Focus on the ability of maintaining short dialogs involving both factoid and personalized content where conversational state has to be maintained.

*	Retrieval: Focus on the ability to identify most likely replies in discussions on external database (i.e. Raddit).

Corpus [Taskmaster-1][1] and [CCPE][2] are in the Woz framework, where users are asked to believe they are interacting with an automated assistant but in fact it is a human agent (with restrictions on coverage and machine controls of interactions to prevents the conversation from becoming overly complex human discourse). In this setting, it can be assumed as creating an idealized spoken environment, revealing how users would openly express themselves with an automated assistant that provided superior natural language understanding. 

There are other some corpus like [Taskmaster-1][1] and [Redial][3] contains self-dialogs (one-person written dataset) for movie recommendation. While the two-person approach to data collection creates a realistic scenario for robust, spoken dialog data collection, this technique is time consuming, complex and expensive, requiring considerable technical implementation as well as administrative procedures to train and manage agents and crowdsourced workers.  An alternative self-dialog approach is in which crowdsourced workers write the full dialogs themselves (i.e. play roles of both user and assistant) on the Amazon Mechanical Turk. They are asked to follow task outline and imagine a scenario which they are speaking to their assistant on the phone and the assistant the given tasks for movies. 

Another kind of corpus like [bAbI][4] is simulated from movie dialogs with automated grammar. The dataset is built from the Open Movie Database (OMDb) and MovieLens about movies. It considers the question answering and recommendation in multiple turns. This is regarded as pre-requisite for an end-to-end dialog system supposed to act as a movie recommendation assistant, and by extension a general dialog agent as well. 

For the question answering subtask, entity matching/selection and end-to-end natural language generation approaches show performance improvement in single-turn and multi-turn question answering tasks. While for the recommendation subtask, reinforcement learning has been proved effective for leaning policy for dialog acts designing reward functions.

### Task 4 The In-Car Navigation Task

<p align="center">
  <img src="https://github.com/youlandasu/service_dialogue/blob/master/navigation.png">
</p>

*Example of the Car Assistant corpus*

We notice that in-car navigation is a constrained scenario which has direct short queries and rely highly on external database. The [InCar Assistant][15] dataset, like the settings of [Stanford Multi-Turn-Multi-Domain][6] dataset, users were presented with the dialogue history up to that point in the running dialogue. A private knowledge base known only to the Car Assistant with information could be useful for satisfying the Driver query. Knowledge bases include a collection of nearby points-of-interest (POI) with relevant information. The Car Assistant is then responsible for using this private information to provide a single utterance that progressed the user-directed dialogues. The Car Assistant is also asked to fill in dialogue state information for mentioned slots and values in the dialogue history up to that point (Eric et al., 2017). 

Such that, the task-oriented dialogue system needs to understand the location type within the query and then retrieve the nearest address. The way for a retriever to collect information from the database should ensure the consistency of giving useful information for navigation. For example, a dialogue system will produce a response with conflict entities if it includes the point of interest (POI) in the fourth row and the address in the fifth row, like “Valero is located at 899 Ames Ct”. 

Neural models for dialogue management explicitly modelled dialogue state through belief and user intent trackers (Wen et al., 2016b; Dhingra et al., 2016) or implicit modelling of dialogue state, forming end-to-end trainable system (Eric et al., 2017). This in-car navigation task addresses retrieval ability of the dialogue system. some neural task-oriented dialogue agents that query underlying knowledge bases and extract relevant entities either do the following: 1) create and execute well-formatted API calls to the KB, operations which require intermediate supervision in the form of training slot trackers and which break differentiability (Wen et al., 2016), or 2) softly attend to the KB and combine this probability distribution with belief trackers as state input for a reinforcement learning policy (Dhingra et al., 2016).

### Task 5 The Restaurant Searching Task 

For a searching task, the system is supposed to retrieving matching entities from the knowledgebase so as to display optional choices based on user’s query and knowledgebase. The semantic representation generated by NLU is further processed by the dialogue management component. A typical dialogue management component includes two stages: dialogue state tracking and policy learning. Dialogue state tacking aims to determine at each turn of a dialogue the full representation of what the user wants at that point in the dialogue, consisting of a goal constraint, a set of requested slots, and dialogue act. 

Wen et al. proposed a Wizard-of-Woz dataset [CamRest676][7] experiment on Amazon MTurk  (2017). CamRest676’s dialogs are in the single domain of restaurant searching, whose knowledge-base(KB) is formed by restaurant, cuisine, address and phone. Each dialogue contains a goal label and several exchanges between a customer and the system. Each user turn was labelled by a set of slot-value pairs representing a coarse representation of dialogue state.

<p align="center">
  <img src="https://github.com/youlandasu/service_dialogue/blob/master/restaurant.png">
</p>

*Knowledge base of the CamRest676 dataset*

Two major problems for restaurant searching are belief state tracking and matching dialogue states with KB’s response, which can be multiple matches, exact match and no match. Encoder-decoder model can be exploited where the encoder represents dialogue state with slot-value pairs (Wen et al., 2017) or in text spans (Lei et al., 2019); and the decoder uses policy network to learn dialogue policy and updating dialogue states as well. Performance is evaluated by BLEU for the language quality of generated response, entity match rate for correctly searching the indicated entities of the user (whether the system answered all the requested information e.g. address, phone number), and F1 for the rate of task completion.

### Task 6 The Service Navigation Task

A conventional kind of applications of task-oriented dialog systems is to execute an action based on domain specification as a user querying an order, where the machine needs to acquire missing information from users through a dialog, and provides the correct information to users once sufficient intelligence is gathered. The so called automatic call distributor or ACD system guides the service entirely from system intuitive, which uses rule-based approaches for extracting information and display a set of options for user to choose from. Its application in call routing significantly limit the variations of response and can only handle simple and most common cases. Commonly, a user is only allowed to respond choosing from finite options, and there are only a few of service-related keywords are detected for system navigating to a sub-service. 

Today, SMS messages, chat and messaging requests, support tickets, leads, or even machine data are routed based on pre-set attributes, such as skills required by the agent, time zone or language preferences, and priority of the task. This process is often referred to as attribute-based routing, which matches tasks to the people or processes that can best handle them. Customers report 2 main problems when communicating with customer services as having trouble contacting right and agent wasting too much time to clarify the problem. Natural language technologies for phone call clustering, key words detection, matching and classification enable routing systems precisely interpret the data.

Dialed Number Identification Service (DNIS) is used to understand which unit the customer should speak to.  The Automatic Number Identification (ANI) is a feature for automatically determining the origination telephone number. This is a good solution for organizations working in different times zones to assign caller to available contact centers or agents that are flagged as available for that particular day and time. Caller input is the most critical information to ensure precision routing. Interactive Voice Response (IVR) systems allow customers to input their intent either by pressing buttons (Dual-tone multi-frequency signaling DTMF) or voice. This data helps identify customer call intent. After caller is identified, intelligent routing systems use detailed information about customer including purchase history, account standing, service agreements and programs, support history and even personal information to ensure that customer intent is accurately understood. Additionally, customer sentiment data can offer clues as to which agent can be more effective in helping the customer. For example, a customer who is always angry in calls should probably be routed to an experienced agent.

### Task 7 The Online Shopping Task

Dialogue agent for online shopping leverage information and execute on it by extracting, understanding and response to customers’ actions. Somehow different from completing a structured order, online shopping agent should monitor meta-data to make recommendations during the conversation, for guiding customers to their wanted products. In spite of natural language processing techniques, the dialogue agent should be able to answer factoid questions (i.e. price, characters, categories) and recommend related products from product database.

While public dialogue corpus for online shopping is rare, researchers aims to learn task-oriented dialogue systems towards different actions in the online shopping scenario, including recommendations, comparison, question answering, opinion summary, proactive questioning and personalized answers. Above actions can be triggered based on dialogue state tracking and then be handled via KB-QA method or templates respectively (Yan et al., 2017). Transfer learning and reinforcement learning framework can also be used for dialogue state tracking and policy learning modeling dialogue as POMDP (Mo et al., 2018), which expands the effectiveness of dialogue management for shopping domains with insufficient data.

### Task 8 The Travel Planning Task

One of the most popular travel planning tasks is ATIS task by DARPAR since 1970s. ATIS task poses problem of user noise, out-of-vocabulary words and grammatically ill-formed utterances, users are asked to perform the requirements getting information (by voice) from the Air Travel database, which contains information about flights fares, airports, aircrafts, etc. The general understander system (GUS) first introduced in 1977 for airline travel planning and management and has been widely used and underlying most modern commercial personal assistants. For ATIS and other travel planning tasks, extracting name entities related to travel are mainly concerned for this kind of task. 

[MultiWOZ][14] Corpus contain dialogs between a tourist and a clerk for traveling planning task, with a goal label and several turns between a user and the system; each system turn has labels from the set of slot-value pairs representing a coarse representation of dialogue state for both user and system (Budzianowski et al. 2018). [Maluuba Frames][13] dataset present a task of frame tracking (El Asri et al., 2017). To choose a trip, users and machines talked about different possibilities, compared them and went back-and-forth between cities, dates, or vacation packages. Frame tracking consists of keeping in memory all the different sets of constraints mentioned by the user. It is a generalization of the state tracking task to a setting where not only the current frame is memorized. 

For single turn travel planning task (i.e. ATIS), dialogue systems are developed especially for speech recognition and spoken language understanding (SLU). SLU consists of contains two essential subtasks which are utterance-level intent detection and slot filling related to name entities. Both speech recognition and spoken language understanding have achieved high accuracy on single-turn dialogues for travel planning, such that can be conceived as the fundamental for dialogue management. For multi turn travel planning task (i.e. MultiWOZ), dialogue management is mainly focused on dialogue management, especially dialogue state tracking. Approaches are either using generic encoders by employing recurrent and self-attention for each utterance and previous system actions (Zhong et al., 2018), or using conditional encoders on slot type embedding vectors (Nouri et al., 2018), and measuring similarity of these computed representation to each slot-value.

### Task 9 The Ticket/Restaurant Booking Task

In a ticket/restaurant booking scenario, conducting goal-oriented dialog requires skills that go beyond language modeling, e.g., asking questions to clearly define a user request, querying Knowledge Bases (KBs), interpreting results from queries to display options to users or completing a transaction. Similar with travel planning, here a user converses with the system to ask for booking tickets, and system could store this as an order format. In this sense, the dialogue system aims to extracting related entities of location and time, which are most crucial for this task. A set of slots specifies what the system needs to know, and the filler of each slot is constrained to values of a particular semantic type. The performance is judged on a binary outcome (success or failure) at the end of each conversation, based on 1) whether a ticket is booked, and 2) whether the booked ticket satisfies the constraints requested by the user.

The [Microsoft Dialogue Challenge][8] contains human-annotated conversational data in three domains (movie-ticket booking, restaurant reservation, and taxi booking). It proposes experimentation platform consisting of a user simulator that mimics a human user and a dialogue system. In the user simulator, an agenda-based user modeling component works at the dialog-act level and controls the conversation exchange conditioned on the generated user goal, to ensure that the user behaves in a consistent, goal-oriented manner. A natural language understanding (NLU) module will process user’s natural language input into a semantic frame. A natural language generation (NLG) module is used to generate natural language sentences responding to the user’s dialogue actions. [Dialog bAbI tasks][9] data is grounded with an underlying KB of restaurant booking and their properties (location, type of cuisine, etc.), these tasks cover several dialog stages and test if models can learn various abilities such as performing dialog management, querying KBs, interpreting the output of such queries to continue the conversation or dealing with new entities not appearing in dialogs from the training set, requires manipulating both natural language and symbols from a KB.

To handle above problems, rule-based approaches, classical information retrieval (IR) models such as TF-IDF match or nearest neighbor (Ritter et al., 2011; Sordoni et al., 2015). Supervised embedding models cast the idea of predicting the next response given the previous conversation using training data consisting of dialogs trained with a margin ranking loss (Dodge et al., 2016). Recurrent neural network (RNN) memory networks have been applied to a range of natural language processing tasks, including question answering (Weston et al., 2015), language modeling (Sukhbaatar et al., 2015), and non-goal-oriented dialog (Dodge et al., 2016). By writing and iteratively reading from a memory component at utterance level, memory networks can store historical dialogs and short-term context to reason about the required response and perform other end-to-end approaches.

### Task 10 The Weather Information Task  

Querying weather information has wide applications in daily scenarios, such as personal assistants on smartphones (e.g. Siri, Cortana), home controllers (e.g. Alexa, Google Home) and in-car assistants. The most related work for getting weather information task is question answering (QA). A simple such systems generally handle a single turn from the user, acting more like question-answering or command-and-control systems. Early work on one-turn question answering retrieval which only matches the last user question with every question in the database using semantic relatedness measures, e.g. BM-25. There has also been research effort in extending nearest neighbor approach to multi-turn dialogs by developing dialog context features extracted from long-term dialog history. 

Task-oriented corpus contains dialogues of weather information are from [Stanford][15], [SNIPS][16] and [Facebook][17], respectively. containing four slot types i.e. location, weekly time, temperature, weather attribute). The latter two corpus aims at augmenting analysis on spoken language understanding systems trained on small amounts of domain-specific text data. Question answering datasets such as question answering in context (QuAC) (Choi et al., 2018) and conversational question answering (CoQA) (Reddy et al., 2016), also challenge the ability of a machine to understand a text passage and answer a series of interconnected questions that appear in a conversation.

### Task 11 The Automatic Tutoring Task 

Automatic tutoring is a task that a machine tries to guide a user to follow a specific scheme of tutoring, exchanging information, response accurately and evaluate correctness. Pipeline methods can be used in this much more complex domains like automatic tutoring. In this example part of a dialog from the adaptive ITSPOKE dialog system (Forbes-Riley et al.,2011), the system detects the hesitancy of the student’s first response (“Is it 19.6 m/s?”), and decides to explain the answer and ask a follow-up question before moving on. ITSPOKE consists of components including automatic speech recognizer, syntactic and semantic analysis, discourse and domain processing, and finite-state dialogue management. Besides the common task for task-oriented dialogue systems that iteratively tracking belief states, the automatic tutoring task also address the issue of comparison and guidance. 

<p align="center">
  <img src="https://github.com/youlandasu/service_dialogue/blob/master/ITSPOKE.png">
</p>

*The tutoring domain: An excerpt from a tutorial interaction between a physics
student and the ITSPOKE system*

The automatic tutoring follows the scheme much more similar with Wizard-of-Oz settings (Budzianowski et al., 2018) or graph knowledge settings (Wu et al., 2019), although they are not meant for task-oriented dialogues. In this scenario, the dialogue agent restricts on coverage and machine controls of interactions to prevents the conversation from becoming overly complex human discourse, following the ontology defining all entity attributes called slots and all possible values for each slot. Different from common task-oriented dialogues, this setting usually encouraging user goal changes. Thus, dialogue states updating ability is highly addressed, and corpus for casual topics can be used for aiding evaluation, for example, [AMI meeting][18] corpus and [Baidu][19] graph knowledge corpus.

### References:

Wei, Zhongyu, Qianlong Liu, Baolin Peng, Huaixiao Tou, Ting Chen, Xuan-Jing Huang, Kam-Fai Wong, and Xiang Dai. "Task-oriented dialogue system for automatic diagnosis." In Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Volume 2: Short Papers), pp. 201-207. 2018.

Eric, Mihail, and Christopher D. Manning. "Key-value retrieval networks for task-oriented dialogue." arXiv preprint arXiv:1705.05414 (2017).

Wen, Tsung-Hsien, David Vandyke, Nikola Mrksic, Milica Gasic, Lina M. Rojas-Barahona, Pei-Hao Su, Stefan Ultes, and Steve Young. "A network-based end-to-end trainable task-oriented dialogue system." arXiv preprint arXiv:1604.04562 (2016).

Lei, Wenqiang, Xisen Jin, Min-Yen Kan, Zhaochun Ren, Xiangnan He, and Dawei Yin. "Sequicity: Simplifying task-oriented dialogue systems with single sequence-to-sequence architectures." In Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pp. 1437-1447. 2018.

Yan, Zhao, Nan Duan, Peng Chen, Ming Zhou, Jianshe Zhou, and Zhoujun Li. "Building task-oriented dialogue systems for online shopping." In Thirty-First AAAI Conference on Artificial Intelligence. 2017.

Mo, Kaixiang, Yu Zhang, Shuangyin Li, Jiajun Li, and Qiang Yang. "Personalizing a dialogue system with transfer reinforcement learning." In Thirty-Second AAAI Conference on Artificial Intelligence. 2018.

Bordes, Antoine, Y-Lan Boureau, and Jason Weston. "Learning end-to-end goal-oriented dialog." arXiv preprint arXiv:1605.07683 (2016).

Ritter, Alan, Colin Cherry, and William B. Dolan. "Data-driven response generation in social media." In Proceedings of the conference on empirical methods in natural language processing, pp. 583-593. Association for Computational Linguistics, 2011.

Sordoni, Alessandro, Michel Galley, Michael Auli, Chris Brockett, Yangfeng Ji, Margaret Mitchell, Jian-Yun Nie, Jianfeng Gao, and Bill Dolan. "A neural network approach to context-sensitive generation of conversational responses." arXiv preprint arXiv:1506.06714 (2015).

Dodge, Jesse, Andreea Gane, Xiang Zhang, Antoine Bordes, Sumit Chopra, Alexander Miller, Arthur Szlam, and Jason Weston. "Evaluating prerequisite qualities for learning end-to-end dialog systems." arXiv preprint arXiv:1511.06931 (2015).

Weston, Jason, Antoine Bordes, Sumit Chopra, Alexander M. Rush, Bart van Merriënboer, Armand Joulin, and Tomas Mikolov. "Towards ai-complete question answering: A set of prerequisite toy tasks." arXiv preprint arXiv:1502.05698 (2015).

Sukhbaatar, Sainbayar, Jason Weston, and Rob Fergus. "End-to-end memory networks." In Advances in neural information processing systems, pp. 2440-2448. 2015.

Liu, Chunyi, Peng Wang, Jiang Xu, Zang Li, and Jieping Ye. "Automatic Dialogue Summary Generation for Customer Service." In Proceedings of the 25th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining, pp. 1957-1965. 2019.

Asri, Layla El, Hannes Schulz, Shikhar Sharma, Jeremie Zumer, Justin Harris, Emery Fine, Rahul Mehrotra, and Kaheer Suleman. "Frames: A corpus for adding memory to goal-oriented dialogue systems." arXiv preprint arXiv:1704.00057 (2017).

Budzianowski, Paweł, Tsung-Hsien Wen, Bo-Hsiang Tseng, Inigo Casanueva, Stefan Ultes, Osman Ramadan, and Milica Gašić. "Multiwoz-a large-scale multi-domain wizard-of-oz dataset for task-oriented dialogue modelling." arXiv preprint arXiv:1810.00278 (2018).

Zhong, Victor, Caiming Xiong, and Richard Socher. "Global-locally self-attentive dialogue state tracker." arXiv preprint arXiv:1805.09655 (2018).

Nouri, Elnaz, and Ehsan Hosseini-Asl. "Toward scalable neural dialogue state tracking model." arXiv preprint arXiv:1812.00899 (2018).

Forbes-Riley, K. and Litman, D. J. (2011). Benefits and challenges of real-time uncertainty detection and adaptation in a spoken dialogue computer tutor. Speech Communication, 53(9), 1115–1136.

Reddy, Siva, Danqi Chen, and Christopher D. Manning. "Coqa: A conversational question answering challenge." Transactions of the Association for Computational Linguistics 7 (2019): 249-266.

Choi, Eunsol, He He, Mohit Iyyer, Mark Yatskar, Wen-tau Yih, Yejin Choi, Percy Liang, and Luke Zettlemoyer. "Quac: Question answering in context." arXiv preprint arXiv:1808.07036 (2018).

Dhingra, Bhuwan, Lihong Li, Xiujun Li, Jianfeng Gao, Yun-Nung Chen, Faisal Ahmed, and Li Deng. "Towards end-to-end reinforcement learning of dialogue agents for information access." arXiv preprint arXiv:1609.00777 (2016).

Wu, Wenquan, Zhen Guo, Xiangyang Zhou, Hua Wu, Xiyuan Zhang, Rongzhong Lian, and Haifeng Wang. "Proactive human-machine conversation with explicit conversation goals." arXiv preprint arXiv:1906.05572 (2019).

[1]: https://research.google/tools/datasets/taskmaster-1/

[2]: https://storage.googleapis.com/dialog-data-corpus/CCPE-M-2019/landing_page.html	

[3]: https://redialdata.github.io/website/

[4]: https://research.fb.com/downloads/babi/

[5]: http://muzhi.baidu.com/

[6]: https://nlp.stanford.edu/blog/a-new-multi-turn-multi-domain-task-oriented-dialogue-dataset/

[7]: https://www.repository.cam.ac.uk/handle/1810/260970

[8]: https://github.com/xiul-msr/e2e_dialog_challenge/tree/master/data

[9]: https://research.fb.com/downloads/babi/

[11]: https://nlds.soe.ucsc.edu/node/30

[12]: http://groups.inf.ed.ac.uk/ami/corpus/

[13]: https://www.microsoft.com/en-us/research/project/frames-dataset/

[14]: http://dialogue.mi.eng.cam.ac.uk/index.php/corpus/

[15]: http://nlp.stanford.edu/projects/kvret/kvret_dataset_public.zip

[16]: https://github.com/snipsco/nlu-benchmark

[17]: https://fb.me/multilingual_task_oriented_data

[18]: http://groups.inf.ed.ac.uk/ami/corpus/

[19]: https://github.com/baidu/Dialogue



