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
