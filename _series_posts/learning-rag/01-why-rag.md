---
title: Why RAG?
series: learning-rag
order: 1
date: 2026-09-10
tags: [rag, llm, ai]
---

Why should we want to learn RAG? How is it related to LLMs? What even is an LLM? This is a first exploration of a few basic concepts that we are going to need later on.

RAG is a good idea because an LLM cannot magically know every private document, and its training data eventually becomes outdated. RAG lets us retrieve useful information at the time of the question and give it to the LLM as context, so it can answer using information it did not originally have.

## What is an LLM?

An LLM is a language model that estimates the probability of the next token given the tokens that came before it. Do you remember talking to a friend, forgetting a specific word and having your friend complete the sentence based on what you were saying and its context? That's kinda it.

For example:

```plaintext
When I hear rain on my roof, I _________ in my kitchen.

```

If we fed this input to our model, the output would be a probability table with the most likely tokens:

| Probability | Token(s)         |
|------------:|:-----------------|
| 9.4%        | cook soup        |
| 5.2%        | warm up a kettle |
| 3.6%        | cower            |
| 2.5%        | nap              |
| 2.2%        | relax            |
{: .fit-first-column }


Note that a token is the atomic unit processed by the model. It can be a whole word, part of a word, a character or even punctuation. A sentence, paragraph or entire essay is therefore represented as a sequence of tokens. The model can also keep generating tokens one after another, so an output does not need to stop after a single token.


One could think the only use case for LLMs is predicting the next token, but this can be extrapolated to other tasks such as:

- Generating text.

- Translating text from one language to another.

- Summarizing documents.


By modelling the statistical patterns between tokens, these models can build useful internal representations and generate plausible language.


### N-gram language models

N-grams are ordered sequences of words used to build language models, where the value $$N$$ is the number of words found in each sequence.

Imagine you are given the sentence *you are very nice*. How many 2-grams (or bigrams) can you build? You can think of it as a sliding window: you can build $$L - N + 1$$ grams, where $$N$$ is the number of words per group and $$L$$ is the length of the input sequence.

In this case it's:

- you are
- are very
- very nice


If we wanted to do a 3-gram then we would be able to generate $$4 - 3 + 1 = 2$$:

- you are very
- are very nice


Imagine we give two words to our 3-gram language model so it can predict the third one: *orange is ____*. It will examine the 3-grams derived from its training corpus that start with *orange is*. Assume it found these two possibilities:

- orange is ripe
- orange is vibrant

The first option most likely refers to the fruit, while the second could refer to the colour. The model only knows which continuation was more common in its training data; it does not yet have much context to help it choose.


## Context

Context is also very important for these models. You are probably already familiar with the idea: while watching episodes of a TV series across several days, you still remember which character is which and part of their history from the first episode where they appeared.

For these models, context is the information available when deciding which token should come next. For example, if we were talking about healthy food and reused the previous orange example, the model should assign more probability to a continuation about the fruit.

Without that context, it has less information and must rely more heavily on patterns from its training data. This does not mean it will go exactly 50/50: each continuation receives whatever probability the learned patterns suggest. With a 3-gram it is very hard to provide much context because the prediction only considers the previous two words.

One could then think that longer N-grams always mean more context and a more accurate model. As $$N$$ grows, though, each exact sequence occurs less often. Eventually most sequences appear only once or not at all, so the model has very little evidence from which to estimate the next word. This is known as data sparsity.

### Recurrent neural networks

Recurrent neural networks can provide more context than N-grams. An RNN processes a sequence one token at a time and passes a hidden state from one step to the next. That state acts like a running memory of what it has seen so far.

In theory an RNN can carry context across a long passage. In practice, useful information can fade as the sequence grows, although gated variants such as LSTMs and GRUs help with this.


Even if these networks can learn more context than N-grams, their useful context is still relatively limited. They must process tokens sequentially, while a Transformer can compare every token with the rest of the available context through attention and can process the training sequence in parallel. An autoregressive Transformer still generates its answer one token at a time.

Additionally, recurrent neural networks are constrained by the vanishing-gradient problem (this sounds cool, and I will have to explore it properly at some point).


## LLMs and the Transformer


Modern LLMs usually use Transformers, and they can predict tokens much better than N-gram models because:

- Attention gives them more direct access to relevant information across the context.
- Their training can be parallelized much more effectively than recurrent models.
- They can scale to large datasets and parameter counts, although being a Transformer does not automatically mean being large.


### What's a Transformer?

Transformers have become the dominant architecture for plenty of language-related tasks, such as translation:

![A Transformer translating a sentence between English and French]({{ '/assets/images/learning-rag/part-1-foundations/why-rag/simple-txfer.svg' | relative_url }}){: .center-img }


The original encoder-decoder Transformer consists of two main parts:

- Encoder: Reads the input and converts it into a contextual representation.
- Decoder: Generates the output token by token. It can use both the previously generated tokens and the representation produced by the encoder.

So the previous example looks like this:

![A Transformer encoder creating an intermediate representation that the decoder converts into translated text]({{ '/assets/images/learning-rag/part-1-foundations/why-rag/txfer-int-repr.svg' | relative_url }}){: .center-img }



There are also encoder-only models, which are useful for understanding or classifying text, and decoder-only models, which are useful for generating text. Most of the generative LLMs that we will later use for RAG are decoder-only.

### What is self-attention?

Transformers use self-attention to make use of context. This is the main idea from the famous *Attention Is All You Need* paper. Given an input, self-attention tries to answer: *How much should each token affect the representation of this token?*


The *self* part is important: the sequence is attending to itself. Each token produces a query, a key and a value. The query of one token is compared with the keys of the other available tokens, producing weights that decide how much of each value should be mixed into the new representation.

For example, assume each token here is a word and the complete context is a single sentence:

*The animal didn't cross the street because it was too tired.*

There are 11 words. In self-attention, each word can assign a different weight to itself and to the other ten words when building its new representation.

For example, *it* is a pronoun, and pronouns are often ambiguous because they refer to a noun or noun phrase. In this case, does *it* refer to the street or to the animal?

The following picture gives us an intuition for this process. A darker and wider connection represents a larger attention weight:

![Self-attention weights showing which words help interpret the pronoun it in a sentence]({{ '/assets/images/learning-rag/part-1-foundations/why-rag/self-att-example.svg' | relative_url }}){: .center-img }

In this illustrative head, *animal* receives the strongest weight from *it*. That is what we would hope to see for this sentence, but we should be careful: one attention weight alone is not proof of the model's reasoning or a perfect explanation of its final prediction.

Now imagine we change the sentence slightly:

*The animal didn't cross the street because it was too wide.*

It is easy for us to see that *it* now refers to *street*. We would therefore hope that at least some of the model's attention heads give more weight to *street* than to *animal*.


Self-attention can be bidirectional when a token is allowed to attend to tokens on both sides. This is useful for an encoder that needs to understand a complete input. A decoder that generates text uses causal, or unidirectional, self-attention: a token can only attend to itself and earlier tokens, otherwise the model would be able to peek at the answer it is supposed to predict.

One attention head performs this query-key-value calculation across the whole available sequence using its own learned projections.

### What is multi-head multi-layer attention?

A self-attention layer usually contains multiple heads. Each head performs attention across the sequence, and their results are concatenated and projected into the layer's final output.

Why have multiple heads? Because each head has its own learned projections, so different heads can learn to focus on different kinds of relationships. Random initialization helps them start differently, but training is what makes those differences useful.

A complete Transformer stacks multiple layers on top of one another, with the output of one becoming part of the input to the next. This lets the model build progressively more complex and abstract representations. Initial layers may capture basic information and syntax, while deeper layers can combine that information into more nuanced patterns involving meaning and context.


#### Big O

As you have seen, self-attention compares every token with every other available token. If we pull back to Big O notation, this looks like a nested `for` and gives us a quadratic term in the sequence length:

$$O(n^2d)$$

Here, $$n$$ is the number of tokens and $$d$$ is the model's representation size. With $$S$$ stacked attention layers, we can describe the attention work as roughly:

$$O(S \cdot n^2d)$$

Multiple heads usually split the representation dimension between them, so we should not blindly multiply the complexity by the number of heads. Also, this only describes the attention part of the Transformer; the other parts of each layer have their own cost.


## How are LLMs trained?

We will probably never have to train an LLM from scratch. It needs a lot of expertise, data, compute and time.

One of the main requirements is an incredible amount of text, usually filtered, cleaned and transformed into tokens.

The exact objective depends on the model architecture. Encoder models such as BERT commonly use masked-language modelling, where some tokens are intentionally hidden and the model tries to recover them. Most generative LLMs use causal language modelling instead: given all the previous tokens, predict the next one.

For a masked-language model, we could start with this sentence:

 *The residents of the sleepy town weren't prepared for what came next.*

And hide some of its tokens:

 *The ___ of the sleepy town weren't prepared for what ___ next.*


An LLM is a neural network, so it learns through a loss function and backpropagation. The loss is not simply the number of correct tokens. It measures how much probability the model assigned to the correct token: confidently assigning it a low probability produces a larger penalty. Backpropagation then calculates how the parameters should change to reduce that loss.

A Transformer trained to predict missing or next tokens ends up learning patterns and higher-level structure from the data. For example, consider this masked input:

 *Oranges are traditionally ___ by hand. Once clipped from a tree, ___ don't ripen.*

With enough training, the model may learn that *harvested* or *picked* are high-probability matches for the first gap, and *oranges* or *they* for the second one.


## Why are LLMs so large?

Many modern LLMs have billions of parameters, and a few reach much larger scales. More parameters give a model more capacity to learn patterns, but bigger does not automatically mean better. Performance also depends on the quality and quantity of the data, the architecture, the training objective and how much compute was used. A Transformer can also be small; it is the architecture, not a synonym for a huge model.



## But how does an LLM generate text?

We have seen a few cases where a model predicts one or two words, which is something plenty of applications such as Gmail already do. An autoregressive LLM uses the same basic mechanism repeatedly: predict one token, append it to the context and use the updated context to predict the next one. Repeating this process can produce thousands of tokens.

Imagine the following case, a full sentence followed by a completely missing sentence:

 *My dog, Max, knows how to perform many traditional dog tricks. ____ (missing sentence).*

An LLM could generate two probabilities (or more):

| Probability | Word(s)                                                    |
|------------:|:-----------------------------------------------------------|
| 3.1%        | For example, he can sit, stay, and roll over.               |
| 2.9%        | For example, he knows how to sit, stay, and roll over.      |
{: .fit-first-column }


A capable LLM can produce an entire answer to a question this way. The underlying operation is still next-token prediction, but it can produce surprisingly complex behaviour, including steps that look like reasoning. We should not assume this means the model understands or verifies every step, but calling it *just autocomplete* can also hide how much structure it learned during training.

## Benefits of LLMs

LLMs can generate extended, clear and easy-to-understand text. They can also perform some tasks without being fine-tuned specifically for them, especially when the prompt contains instructions or a few examples. This is known as zero-shot or few-shot generalisation, although the result still depends on patterns learned during pretraining.


## Problems with LLMs

Training an LLM involves:

- Gathering a large enough, good-quality training set.
- Consuming a lot of time, energy and compute resources.
- Solving difficult parallelism and infrastructure challenges.


When using an LLM to make predictions:

- It can hallucinate: produce plausible information that is unsupported or wrong.
- Inference can consume significant compute and energy.
- Like any machine-learning model, it can reproduce biases from its data and training process.
- Its internal knowledge can be outdated, and it does not automatically know our private documents.
- Its context window is limited, so we cannot give it an unlimited amount of information at once.

The last three problems are some of the reasons RAG is useful: retrieve a small amount of relevant, current information and place it inside the model's available context.


## Adapting an LLM

A general-purpose model trained on broad data is often called a foundation model, base model or pretrained model.

A foundation LLM learns patterns involving grammar, words, idioms and many of the topics represented in its training data. It can generate useful sentences and can even produce creative text such as poetry. However, its default behaviour will not necessarily solve our specific application's problem.


There is no single required process for turning a foundation model into an application. Depending on the problem, we might use:

- Prompt engineering to describe the task.
- RAG to provide external knowledge at inference time.
- Fine-tuning to change the model's behaviour or specialise it.
- Distillation to train a smaller model using a larger one as a teacher.

These options can be used separately or combined. In this series our focus is obviously going to be RAG.


### Fine-tuning


Foundation LLMs are already strong pattern-recognition systems, so sometimes a smaller amount of additional training can specialise them for a particular task. Fine-tuning uses examples related to the behaviour we want the application to perform, and the required number and quality of examples depends on the task.

Full fine-tuning can be computationally expensive because it updates all the model's parameters. Parameter-efficient fine-tuning, or PEFT, updates or adds only a small subset of parameters, making the process much cheaper.


A fine-tuned model can perform better than the foundation model on the target task, even when it contains the same number of parameters. This is not guaranteed, though: poor data or excessive specialisation can make it worse or damage its performance on other tasks.


### Distillation

Some LLMs end up with many parameters and therefore require a lot of resources to run. We might not need all that capacity for a narrower application.

Distillation is a way to train a smaller student model to imitate a larger teacher model. The student can generate predictions faster and use fewer resources, but it may lose some of the teacher's capabilities or accuracy.

A common strategy is to give inputs to the teacher and collect its output labels, generated answers or probability distributions. Those teacher-produced targets are then used to train the smaller student model. The student is a separate model; we are not simply deleting parameters from the teacher.


### Prompt engineering

Prompt engineering lets users guide the LLM's output without changing its parameters. For example, imagine that we want a model to output the botanical class of a fruit using this format:

```plaintext
fruit: class
```

A one-shot prompt gives the LLM one completed example and then asks it to follow that pattern:


```
peach: drupe
apple:

```

And the LLM responds:

```
apple: pome
```

Sometimes one example is not enough, so we use few-shot prompting:

```
peach: drupe
apple: pome
lemon:
```

We would expect the LLM to answer with the class for `lemon`.


There is also zero-shot prompting, where we provide an instruction but no completed examples:

```plaintext
Return the botanical class of this fruit: apple
```

LLMs usually benefit from clear instructions and useful context, so we should provide them when we can.


## Online and offline inference

Online inference means the model generates a response when a request arrives. This is how interactive chat and most question-answering applications work, including the RAG system we are going to build.

Offline, or batch, inference means running predictions ahead of time for inputs we already know about. For example, we could summarize a fixed collection of documents overnight. This can be cheaper and easier to schedule, but it cannot answer a new question that was not known in advance.

Caching is related but is not the same thing. A cache lets us reuse a previous result when the same or a sufficiently similar request appears again. Most LLM applications use online inference, sometimes combined with batching and caching where those techniques make sense.

## Glossary

- Token: The atomic unit processed by a language model. Depending on the tokenizer, it can represent a word, part of a word, a character or punctuation. Modern LLMs usually use subword tokens, for example:

    - A word-level tokenizer could represent `dogs like cats` as `dogs`, `like`, `cats`.
    - A character-level tokenizer could represent `bike fish` using 9 tokens, including the space: `b`, `i`, `k`, `e`, ` `, `f`, `i`, `s`, `h`.
    - A subword tokenizer might split `taller` into `tall` and `er`, or `unwatched` into `un`, `watch` and `ed`. The exact split depends on the tokenizer; it does not have to follow grammatical prefixes and suffixes perfectly.


- Recurrent neural network: A neural network that processes a sequence step by step and passes a hidden state from one step into the next.

- Neural network: A model made from connected layers of units that learn parameters from data. A network with multiple hidden layers is usually called a deep neural network.

- Hidden layer: A layer between the input and output of a neural network. It transforms the representation before passing it forward.

- Neuron: A unit that calculates a weighted combination of its inputs and usually passes the result through an activation function.

- Attention: A mechanism that assigns weights to available representations and combines them according to their relevance for the current calculation. It does not necessarily compress the information.

- Self-attention: A mechanism that transforms a sequence of representations into another sequence of contextual representations. Each output integrates information from the allowed elements of the same input sequence. The *self* in the name refers to the sequence attending to itself rather than to a different source.


## Resources

- [Introduction to Large Language Models](https://developers.google.com/machine-learning/crash-course/llm).
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/).
- [Vanishing gradient problem](https://developers.google.com/machine-learning/glossary#vanishing-gradient-problem).
