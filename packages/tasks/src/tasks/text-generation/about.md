This task covers guides on both [text-generation](https://huggingface.co/models?pipeline_tag=text-generation&sort=downloads) and [text-to-text generation](https://huggingface.co/models?pipeline_tag=text2text-generation&sort=downloads) models. Popular large language models that are used for chats or following instructions are also covered in this task. You can find the list of selected open-source large language models [here](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard), ranked by their performance scores.

## Use Cases

### Instruction Models

A model trained for text generation can be later adapted to follow instructions. One of the most used open-source models for instruction is OpenAssistant, which you can try [at Hugging Chat](https://huggingface.co/chat).

### Code Generation

A Text Generation model, also known as a causal language model, can be trained on code from scratch to help the programmers in their repetitive coding tasks. One of the most popular open-source models for code generation is StarCoder, which can generate code in 80+ languages. You can try it [here](https://huggingface.co/spaces/bigcode/bigcode-playground).

### Stories Generation

A story generation model can receive an input like "Once upon a time" and proceed to create a story-like text based on those first words. You can try [this application](https://huggingface.co/spaces/mosaicml/mpt-7b-storywriter) which contains a model trained on story generation, by MosaicML.

If your generative model training data is different than your use case, you can train a causal language model from scratch. Learn how to do it in the free transformers [course](https://huggingface.co/course/chapter7/6?fw=pt)!

## Task Variants

### Completion Generation Models

A popular variant of Text Generation models predicts the next word given a bunch of words. Word by word a longer text is formed that results in for example:

- Given an incomplete sentence, complete it.
- Continue a story given the first sentences.
- Provided a code description, generate the code.

The most popular models for this task are GPT-based models, [Mistral](mistralai/Mistral-7B-v0.1) or [Llama series](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf). These models are trained on data that has no labels, so you just need plain text to train your own model. You can train text generation models to generate a wide variety of documents, from code to stories.

### Text-to-Text Generation Models

These models are trained to learn the mapping between a pair of texts (e.g. translation from one language to another). The most popular variants of these models are [NLLB](facebook/nllb-200-distilled-600M), [FLAN-T5](https://huggingface.co/google/flan-t5-xxl), and [BART](https://huggingface.co/docs/transformers/model_doc/bart). Text-to-Text models are trained with multi-tasking capabilities, they can accomplish a wide range of tasks, including summarization, translation, and text classification.

## Language Model Variants

When it comes to text generation, the underlying language model can come in several types:

- **Base models:** refers to plain language models like [Mistral 7B](mistralai/Mistral-7B-v0.1) and [Llama-2-70b](https://huggingface.co/meta-llama/Llama-2-70b-hf). These models are good for fine-tuning and few-shot prompting.

- **Instruction-trained models:** these models are trained in a multi-task manner to follow a broad range of instructions like "Write me a recipe for chocolate cake". Models like [Flan-T5](https://huggingface.co/google/flan-t5-xl), [Mistral-7B-Instruct-v0.1](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.1), and [falcon-40b-instruct](https://huggingface.co/tiiuae/falcon-40b-instruct) are examples of instruction-trained models. In general, instruction-trained models will produce better responses to instructions than base models.

- **Human feedback models:** these models extend base and instruction-trained models by incorporating human feedback that rates the quality of the generated text according to criteria like [helpfulness, honesty, and harmlessness](https://arxiv.org/abs/2112.00861). The human feedback is then combined with an optimization technique like reinforcement learning to align the original model to be closer with human preferences. The overall methodology is often called [Reinforcement Learning from Human Feedback](https://huggingface.co/blog/rlhf), or RLHF for short. [Llama2-Chat](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf) is an open-source model aligned through human feedback.

## Text Generation from Image and Text

There are language models that can input both text and image and output text, called vision language models. [LLaVA](https://huggingface.co/llava-hf/llava-1.5-7b-hf) and [BLIP-2](https://huggingface.co/Salesforce/blip2-opt-2.7b) are good examples. Although they work just like other language models by means of input parameters for generation, since they also take input images, you can use them with `image-to-text` pipeline. You can find information about the pipeline in [image-to-text](https://huggingface.co/tasks/image-to-text) task page.

## Inference

You can use the 🤗 Transformers library `text-generation` pipeline to do inference with Text Generation models. It takes an incomplete text and returns multiple outputs with which the text can be completed.

```python
from transformers import pipeline
generator = pipeline('text-generation', model = 'HuggingFaceH4/zephyr-7b-beta')
generator("Hello, I'm a language model", max_length = 30, num_return_sequences=3)
## [{'generated_text': "Hello, I'm a language modeler. So while writing this, when I went out to meet my wife or come home she told me that my"},
##  {'generated_text': "Hello, I'm a language modeler. I write and maintain software in Python. I love to code, and that includes coding things that require writing"}, ...
```

[Text-to-Text generation models](https://huggingface.co/models?pipeline_tag=text2text-generation&sort=downloads) have a separate pipeline called `text2text-generation`. This pipeline takes an input containing the sentence including the task and returns the output of the accomplished task.

```python
from transformers import pipeline

text2text_generator = pipeline("text2text-generation")
text2text_generator("question: What is 42 ? context: 42 is the answer to life, the universe and everything")
[{'generated_text': 'the answer to life, the universe and everything'}]

text2text_generator("translate from English to French: I'm very happy")
[{'generated_text': 'Je suis très heureux'}]
```

You can use [huggingface.js](https://github.com/huggingface/huggingface.js) to infer text classification models on Hugging Face Hub.

```javascript
import { HfInference } from "@huggingface/inference";

const inference = new HfInference(HF_TOKEN);
await inference.conversational({
	model: "distilbert-base-uncased-finetuned-sst-2-english",
	inputs: "I love this movie!",
});
```

## Text Generation Inference

[Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) is an open-source toolkit for serving LLMs tackling challenges such as response time. TGI powers inference solutions like [Inference Endpoints](https://huggingface.co/inference-endpoints) and [Hugging Chat](https://huggingface.co/chat/), as well as multiple community projects. You can use it to deploy any supported open-source large language model of your choice.

## ChatUI Spaces

Hugging Face Spaces includes templates to easily deploy your own instance of a specific application. [ChatUI](https://github.com/huggingface/chat-ui) is an open-source interface that enables serving conversational interface for large language models and can be deployed with few clicks at Spaces. TGI powers these Spaces under the hood for faster inference. Thanks to the template, you can deploy your own instance based on a large language model with only a few clicks and customize it. Learn more about it [here](https://huggingface.co/docs/hub/spaces-sdks-docker-chatui) and create your large language model instance [here](https://huggingface.co/new-space?template=huggingchat/chat-ui-template).

![ChatUI](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/os_llms/docker_chat.png)

## Useful Resources

Would you like to learn more about the topic? Awesome! Here you can find some curated resources that you may find helpful!

### Tools within Hugging Face Ecosystem

- You can use [PEFT](https://github.com/huggingface/peft) to adapt large language models in efficient way.
- [ChatUI](https://github.com/huggingface/chat-ui) is the open-source interface to conversate with Large Language Models.
- [text-generation-inferface](https://github.com/huggingface/text-generation-inference)
- [HuggingChat](https://huggingface.co/chat/) is a chat interface powered by Hugging Face to chat with powerful models like Llama 2 70B.

### Documentation

- [PEFT documentation](https://huggingface.co/docs/peft/index)
- [ChatUI Docker Spaces](https://huggingface.co/docs/hub/spaces-sdks-docker-chatui)
- [Causal language modeling task guide](https://huggingface.co/docs/transformers/tasks/language_modeling)
- [Text generation strategies](https://huggingface.co/docs/transformers/generation_strategies)

### Course and Blogs

- [Course Chapter on Training a causal language model from scratch](https://huggingface.co/course/chapter7/6?fw=pt)
- [TO Discussion with Victor Sanh](https://www.youtube.com/watch?v=Oy49SCW_Xpw&ab_channel=HuggingFace)
- [Hugging Face Course Workshops: Pretraining Language Models & CodeParrot](https://www.youtube.com/watch?v=ExUR7w6xe94&ab_channel=HuggingFace)
- [Training CodeParrot 🦜 from Scratch](https://huggingface.co/blog/codeparrot)
- [How to generate text: using different decoding methods for language generation with Transformers](https://huggingface.co/blog/how-to-generate)
- [Guiding Text Generation with Constrained Beam Search in 🤗 Transformers](https://huggingface.co/blog/constrained-beam-search)
- [Code generation with Hugging Face](https://huggingface.co/spaces/codeparrot/code-generation-models)
- [🌸 Introducing The World's Largest Open Multilingual Language Model: BLOOM 🌸](https://huggingface.co/blog/bloom)
- [The Technology Behind BLOOM Training](https://huggingface.co/blog/bloom-megatron-deepspeed)
- [Faster Text Generation with TensorFlow and XLA](https://huggingface.co/blog/tf-xla-generate)
- [Assisted Generation: a new direction toward low-latency text generation](https://huggingface.co/blog/assisted-generation)
- [Introducing RWKV - An RNN with the advantages of a transformer](https://huggingface.co/blog/rwkv)
- [Creating a Coding Assistant with StarCoder](https://huggingface.co/blog/starchat-alpha)
- [StarCoder: A State-of-the-Art LLM for Code](https://huggingface.co/blog/starcoder)
- [Open-Source Text Generation & LLM Ecosystem at Hugging Face](https://huggingface.co/blog/os-llms)
- [Llama 2 is at Hugging Face](https://huggingface.co/blog/llama2)

### Notebooks

- [Training a CLM in Flax](https://github.com/huggingface/notebooks/blob/master/examples/causal_language_modeling_flax.ipynb)
- [Training a CLM in TensorFlow](https://github.com/huggingface/notebooks/blob/master/examples/language_modeling_from_scratch-tf.ipynb)
- [Training a CLM in PyTorch](https://github.com/huggingface/notebooks/blob/master/examples/language_modeling_from_scratch.ipynb)

### Scripts for training

- [Training a CLM in PyTorch](https://github.com/huggingface/transformers/tree/main/examples/pytorch/language-modeling)
- [Training a CLM in TensorFlow](https://github.com/huggingface/transformers/tree/main/examples/tensorflow/language-modeling)
- [Text Generation in PyTorch](https://github.com/huggingface/transformers/tree/main/examples/pytorch/text-generation)
