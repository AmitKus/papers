### [The Reversal Curse: LLMs trained on “A is B” fail to learn “B is A”](https://arxiv.org/pdf/2309.12288v1.pdf)

- If a model is trained on a sentence of the form “A is B”, it will not automatically generalize to the reverse direction “B is A”.

- Example, if a model is trained on “Olaf Scholz was the ninth Chancellor of Germany”, it will not automatically be able to answer the question, “Who was the ninth Chancellor of Germany?”.

- The Reversal Curse is robust across model sizes and model families and is not alleviated by data augmentation.

- Methodology: Fine-tune on made-up data about celebreties and then test the reverse direction to which the model is trained.

- Note: The Reversal Curse does not apply for in-context learning. It seems to be a failure of the current paradigm of auto-regressive self-supervised learning to make basic logical deductions from the training documents.
    - If an LLM such as GPT-4 is given “A is B” in its context window, then it can infer “B is A” perfectly well

### [LMSYS-CHAT-1M: A LARGE-SCALE REAL-WORLD LLM CONVERSATION DATASET](https://arxiv.org/pdf/2309.11998.pdf)

- [LMSYS CHAT APP](https://chat.lmsys.org/)
- [Conversational data](https://huggingface.co/datasets/lmsys/lmsys-chat-1m)
- 25 SOTA LLMs
![](../pics/Screenshot%20from%202023-09-24%2023-06-46.png)
- 210K unitque IP addresses of users
- Most commont question types are coding related
![](../pics/Screenshot%20from%202023-09-24%2023-09-30.png)
- 4 usecases of this dataset:
    1. developing content moderation models, 
    2. building a safety benchmark, 
    3. training instruction-following models,
    4. creating challenging benchmark questions.
- Jail breaking prompts 
    - Content Warning: Ask the model to start the response with a warning of possible explicit contents.
    - Educational Purposes: Frame the harmful question as an educational inquiry or for educational purposes. 
    - Harmful Rewrite: Ask the model to rewrite their responses but with increased toxicity.
    - Misspelling Keywords: Sometimes when keywords in a harmful prompt is misspelled, there is an increased likelihood for the model to comply to the malicious request. 
    - Token Replacement: Define the keyword with another word, mask, or token and replace the keyword in the prompt. Switch the keyword back into the prompt in the later prompts.
    - Translation: Ask the model to translate the harmful input into a foreign language and then ask it to translate it back in some forms.


## [Augmented Language Models: a Survey](https://arxiv.org/abs/2302.07842)

- Survey paper about Augmented Langauge Models
- Traditional LM paradigm: Parametric models predicting the next token probibility
- ALMs: Can use various, possibly non-parametric external modules to expand their context processing ability

### Definitions
- **Reasoning:** In the context of ALMs, reasoning is decomposing a potentially complex task into simpler subtasks the LM can solve more easily by itself or using tools.
- **Tool:** For ALMs, a tool is an external module that is typically called using a rule or a special token and whose output is included in the ALM’s context.
- **Act:** For ALMs, calling a tool having an effect on the virtual or physical world and observing the result, typically by including it in the ALM’s current context.

### Chain of thought example
Wei et al. (2022c) introduced chain-of-thought (CoT), a few-shot prompting technique for LMs.

The ability to perform reasoning with chain-of-thoughts from a few in-context examples only emerges as models reach a certain size

```
Question: Roger has 5 tennis balls. He buys 2 more cans of tennis balls. Each can has 3 tennis balls. How many tennis balls does he have now? 

Answer: Roger started with 5 balls. 2 cans of 3 tennis balls each is 6 tennis balls. 5 + 6 = 11. The answer is 11. 

Question: The cafeteria had 23 apples. If they used 20 to make lunch and bought 6 more, how many apples do they have? 

Answer: <LM>
```

### Iterative prompting example
```
Iteration 0 
Text: Brittney Reese (born September 9, 1986 in Gulfport, Mississippi) is an American long jumper. 
<LM> 

Plan: Remove incorrect information 

Edit: Brittney Reese (born September 9, 1986 in Gulfport, Mississippi) is an American long jumper. 
</LM> 

Iteration 1 
Text: Brittney Reese (born September 9, 1986) is an American long jumper. 
<LM> 
Plan: Add information about her career 
Edit: Brittney Reese (born September 9, 1986) is an American long jumper , who competed at the 2008 Summer Olympics, and is a 4-time World Champion . 
</LM> 

Iteration 2 
Text: Brittney Reese (born September 9, 1986) is an American long jumper, who competed at the 2008 Summer Olympics, and is a 4-time World Champion. 
<LM> 
Plan: Add her birthplace 
Edit: Brittney Reese (born September 9, 1986 in Inglewood, California ) is an American long jumper, who competed at the 2008 Summer Olympics, and is a 4-time World Champion. 
</LM>
```

### Why reinforcement learning
Reinforcement learning Supervised learning from human-created prompts is effective to teach models to reason and act. However, **such data is difficult and costly to obtain. Human preference data — such as rankings or likes/dislikes — is much easier, faster, and cheaper to obtain than full demonstrations.**

### Limitations

Since ALMs only perform predictions at the token level, they cannot reason according to LeCun (2022)’s view and may be still limited to System 1 tasks, i.e. that rely on reflex rather than logic and thinking.


## [Effective Long-Context Scaling of Foundation Models](https://arxiv.org/abs/2309.16039)

### What
- Llama2 exteded sequence length
- 7B/13B: 32,768-token sequences 
- 34B/70B: 16,384-token sequences.
### How
- Continual pretraining from LLAMA with longer training sequences and on a dataset where long texts are upsampled.
- Additional 400B tokens formed as long-training sequences
- chat model: instruction finetune our continually pretrained long models without any human-annotated data.




### Key points

- 70B variant can already surpass gpt-3.5-turbo-16k’s overall performance on a suite of long-context tasks.
- ablation experiments suggest that having abundant long texts in the pretrain dataset is not the key to achieving strong performance
- empirically verify that long context continual pretraining is more efficient and similarly effective compared to pretraining from scratch with long sequences.

![](../pics/Screenshot%20from%202023-10-03%2009-51-54.png)

- not only observe significant improvements on long-context tasks but also modest improvements on standard short-context tasks, especially on coding, math, and knowledge benchmarks.







### Additional

- Longer sequence-length models often overlook the necessity of maintaining strong performance on standard short-context tasks, either bypassing the evaluations or reporting degenerated performance