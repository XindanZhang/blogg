I've been reading this blog written by Qwen Team.

## [Qwen2.5-Max: Exploring the Intelligence of Large-scale MoE Model](https://qwenlm.github.io/blog/qwen2.5-max/)

As it's hard to scale extremely large models, whether they are dense or MoE models.

Qwen2.5-Max, a large-scale MoE model that has been pretrained on over 20 trillion tokens and further post-trained with curated Supervised Fine-Tuning (SFT) and Reinforcement Learning from Human Feedback (RLHF) methodologies.

```python
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.getenv("API_KEY"),
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
)

completion = client.chat.completions.create(
    model="qwen-max-2025-01-25",
    messages=[
      {'role': 'system', 'content': 'You are a helpful assistant.'},
      {'role': 'user', 'content': 'Which number is larger, 9.11 or 9.8?'}
    ]
)

print(completion.choices[0].message)
```
