## Concept of Scaling Law

Scaling Law refers to the relationship between the performance of AI models and and factors such as model size(number of parameters), amount of training data, and computational resources.

> [Scaling_Laws_for_Neural_Language_Models](https://arxiv.org/pdf/2001.08361)
> Performance depends strongly on scale, weakly on model shape: Model performance depends most
strongly on scale, which consists of three factors: the number of model parameters N (excluding embeddings), the size of the dataset D, and the amount of compute C used for training.
## Specific Manifestations of Scaling Law

**Model Size and Performance:**

Generally, large models(with more parameters) have stronger capabilities. Increasing the complexity of the model can enhance its ability to handle tasks.

**Importance of Data Volume**

**Computational Resource**


![[images/Scaling_choices.PNG]]_There is an image showing perfectly the the scaling choices for pretraining._

The above is kind of traditional "scaling laws".

To solve the lack of superb training corpus which we all know is one of the most important parts for enhancing the capability of models' performance, researchers tried to use synthetic data produced by LLMs themselves. But they have some limitations.

## What comes next to scaling law?

Some key notes from [Scaling LLM Test Time Compute](https://www.jonvet.com/blog/llm-test-time-compute)
> At inference or we say test time, scale typically refers to the amount of compute used to generate the final model prediction.

> Test-time compute can refer to many things: creating chains of thought, revising answers, using external verifiers, backtracking, sampling multiple times and selecting the best answer and so on.

[Stratgies](https://huggingface.co/spaces/HuggingFaceH4/blogpost-scaling-test-time-compute) in scaling test-time compute:
> **Self-Refinement:** Models iteratively refine their own outputs or “thoughts” by identifying and correcting errors in subsequent iterations. While effective on some tasks, this strategy usually requires models to have built-in mechanisms for self-refinement, which can limit its applicability.

> **Search Against a Verifier:** This approach focuses on generating multiple candidate answers and using verifier to select the best one. A verifier can be anything from a hard-coded heuristic to a learned reward model, but for the purposes of this blog post we will focus on learned verifiers. It includes techniques such as Best-of-N sampling and tree search. Search strategies are more flexible and can adapt to the difficulty of the problem, although their performance is constrained by the quality of the verifier.

One of the [test-time scaling](https://huggingface.co/spaces/HuggingFaceH4/blogpost-scaling-test-time-compute)methods is to use **Verifier models** like the simplest one Best of N sampling, a.k.a. **rejection sampling**.

- Outcome-Supervised Reward Model (ORM)

- Process-Supervised Reward Models (PRM)
