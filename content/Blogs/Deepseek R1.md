## Key Learnings from Deepseek R1

After reading **[Deepseek R1 for Everyone](https://trite-song-d6a.notion.site/Deepseek-R1-for-Everyone-1860af77bef3806c9db5e5c2a256577d)**, I gained clarity on the reasoning model Deepseek R1.

---

### **Chain-of-Thought Reasoning**
Force the model to think longer rather than just giving us answer.

- CoT involves adding explicit, task-specific cues to a user’s query (e.g., “Show your reasoning step by step”) to structure the output format and encourage the model to articulate its logical process incrementally. This is often used to improve accuracy and transparency for complex problems.

- System prompting, by contrast, defines the overall role or behavior of the AI (e.g., “You are a sarcastic historian”) at the start of an interaction. It sets broad guidelines for tone, expertise, or interaction style, shaping responses globally rather than dictating the format of individual answers.


---

### **Reinforcement Learning**


---

### **GPRO**


---

## Limitations of PRM

Process Reward Model (PRM)

Limitations of PRM: The report identifies three main limitations of PRM that hinder its success in large-scale reinforcement learning:

1.  Defining Fine-Grain Steps: It is challenging to explicitly define fine-grain steps in general reasoning tasks.
2.  Intermediate Step Validation: Determining whether the current intermediate step is correct is difficult. Automated annotation using models may not yield satisfactory results, while manual annotation is not scalable.
3.  Reward Hacking: Introducing a model-based PRM can lead to reward hacking, requiring additional training resources and complicating the training pipeline￼.

## Limitations of MCTS
Monte Carlo Tree Search (MCTS)

Challenges with MCTS: The report discusses the challenges encountered when using MCTS to enhance test-time compute scalability:

1.  Search Space Complexity: Unlike chess, token generation presents an exponentially larger search space, making it difficult to avoid local optima.
2.  Value Model Training: Training a fine-grained value model is inherently difficult, which makes it challenging for the model to iteratively improve￼.
