## Key Learnings from Deepseek R1

**[Deepseek R1 for Everyone](https://trite-song-d6a.notion.site/Deepseek-R1-for-Everyone-1860af77bef3806c9db5e5c2a256577d)**

---

### **Chain-of-Thought Reasoning**
Force the model to think longer rather than just giving us answer.

- CoT involves adding explicit, task-specific cues to a user’s query (e.g., “Show your reasoning step by step”) to structure the output format and encourage the model to articulate its logical process incrementally. This is often used to improve accuracy and transparency for complex problems.

- System prompting, by contrast, defines the overall role or behavior of the AI (e.g., “You are a sarcastic historian”) at the start of an interaction. It sets broad guidelines for tone, expertise, or interaction style, shaping responses globally rather than dictating the format of individual answers.


---

### **Reinforcement Learning**


---

### **GPRO**

The training uses the Group Relative Policy Optimization (GRPO) algorithm, which does not require a separate critic model. Instead, it calculates the baseline from a set of scores. The reward function combines accuracy and format adherence.

---

## [The Multi-Stage Training of DeepSeek R1](https://www.philschmid.de/deepseek-r1)

![[images/deepseek_r1_process.png]]_[The training process of Deepseek R1](https://mirror-feeling-d80.notion.site/DeepSeek-R1-182808527b17801585dadb84f7c66cd9)_

>To prevent the early unstable cold start phase of reinforcement training (RL) training from the base model, the team started with supervised fine-tuning.
>Stage 1/4 Base to Supervised Fine-Tuning (SFT)
>Collected up to 10k token-long chain-of-thought (CoT) using the fine-tuned models, R1-zero and human annotator. The data is used to fine-tune Deepseek V3 base to improve readbility and coherence.
>Stage 2/4 RL for Reasoning
>Used the same RL pipeline as R1-Zero, focusing on reasoning-intensive tasks such as coding and math using the same Rule-Based Reward Models. This time, an additional reward for "language consistency" is used to help the model stick to the same language.
>Stage 3/4 Rejection Sampling and SFT
>Generated large synthetic dataset using Reject Sampling (RS) focusing on writing, role-playing, and other general-purpose tasks. The model from Stage 2 was used with Deepseek V3 as a Judge to generate 600k reasoning-related samples and 200k for writing, role-playing, and other general-purpose tasks using portions of the SFT dataset of DeepSeek-V3 or regenerating them with CoT included.
>Stage 4/4 RL for Helpfulness
>In the Final Stage, GRPO is used again with a combination of Rule-Based and Outcome Reward Models to improve the model's helpfulness and harmlessness. Leading to the Deepseek R1 model.

## Limitations of PRM

**Process Reward Model (PRM)**

Limitations of PRM: The report identifies three main limitations of PRM that hinder its success in large-scale reinforcement learning:

1.  Defining Fine-Grain Steps: It is challenging to explicitly define fine-grain steps in general reasoning tasks.
2.  Intermediate Step Validation: Determining whether the current intermediate step is correct is difficult. Automated annotation using models may not yield satisfactory results, while manual annotation is not scalable.
3.  Reward Hacking: Introducing a model-based PRM can lead to reward hacking, requiring additional training resources and complicating the training pipeline.

## Limitations of MCTS
**Monte Carlo Tree Search (MCTS)**

Challenges with MCTS: The report discusses the challenges encountered when using MCTS to enhance test-time compute scalability:

1.  Search Space Complexity: Unlike chess, token generation presents an exponentially larger search space, making it difficult to avoid local optima.
2.  Value Model Training: Training a fine-grained value model is inherently difficult, which makes it challenging for the model to iteratively improve.


## Distillation

Distillation Success: The report shows that reasoning patterns from larger models can be distilled into smaller models, resulting in better performance compared to reasoning patterns discovered through RL on small models.

## Conclusion

**Deepseek R1-Zero**, this model demonstrates significant reasoning capibilities through pure RL without any supervised fine-tuning data.

**Deepseek R1**, this model incorporates a small amount of cold start data and MST(multi-stage training) pipeline to address the issues in Zero model like language-mixing and poor readability.
