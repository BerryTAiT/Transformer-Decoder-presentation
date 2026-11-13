# Transformer Decoder — Speaker Notes

A slide-by-slide script you can read aloud. Each section is timed to feel natural — pause where marked with [pause].

---

## Slide 1 — Decoder Architecture

"Before we dive into the details, here is the complete picture of what we're going to break down. This is the decoder — the part of the Transformer that actually generates text. You can see the input comes in at the bottom, flows up through several stages, and a predicted word comes out the top. [pause] We're going to walk through every single piece of this, one by one, so by the end you'll understand exactly how ChatGPT produces the next word."

---

## Slide 2 — Pipeline Animation

"This animation shows the whole process playing out from start to finish. Watch as text is typed in, split into tokens, turned into numbers, processed through attention and decoder layers, and finally a word is predicted. [pause] I'll explain each stage individually — this is just to give you a sense of the full flow before we get into the details."

---

## Slide 3 — Cover

"Today we're talking about the Transformer decoder — the architecture behind ChatGPT, GPT-4, Llama, and most modern AI language models. We'll cover what it is, who invented it, and exactly how it works from input to output."

---

## Slide 4 — What is a Transformer

"A Transformer is a type of neural network designed to understand and generate language. What makes it special is something called attention — it lets the model look at all the words in a sentence at the same time, instead of reading them one by one like older models did. [pause] This is why it's so good at understanding context and generating coherent text."

---

## Slide 5 — Who made it

"The Transformer was invented in 2017 by a team at Google Brain, led by Ashish Vaswani. They published a paper called 'Attention Is All You Need.' [pause] Originally it was built for translation — turning English into French and back. But researchers quickly realized the decoder part alone could generate amazing text, and that became GPT."

---

## Slide 6 — Decoder Overview

"Here's the roadmap of everything inside a decoder. The input text goes through: tokenization, embedding, positional encoding, then it enters the decoder blocks which contain masked attention and feed-forward networks, and finally the output head predicts the next word. [pause] Let's go through each step."

---

## Slide 7 — Stage 1 — Tokenization

"First step: tokenization. The model doesn't understand letters or words directly — it works with numbers. So we take the input sentence and split it into small pieces called tokens. [pause] Each token could be a whole word, part of a word, or punctuation. Then we look up each token in a vocabulary list and assign it a number, called a token ID. That's all the model sees — just a sequence of numbers."

---

## Slide 8 — Stage 2 — Embeddings

"Now we have numbers, but a number like 8421 doesn't mean anything by itself. So we look up each token ID in an embedding table — a giant table the model learned during training — and replace the number with a vector: a list of hundreds or thousands of numbers. [pause] Each number in that vector captures some aspect of the word's meaning. Words that are similar end up with similar vectors."

---

## Slide 9 — Embeddings in 3D Space

"This visualization shows what those vectors look like if we plot them in space. Each dot is a word. [pause] Notice how similar words cluster together — all the animals are near each other, all the verbs are together, all the little function words like 'the' and 'on' form their own group. [pause] The model 'understands' meaning by measuring how far apart these points are. Close means related, far means unrelated."

---

## Slide 10 — Stage 3 — Positional Encoding

"There's a problem though: the model processes all tokens at once, so it has no idea what order they came in. 'The cat sat on' would look the same as 'sat on the cat.' [pause] Positional encoding fixes this. We add a unique number pattern to each token's vector based on its position — first word, second word, third word, and so on. [pause] Now the model knows both what each word means AND where it appears in the sentence."

---

## Slide 11 — Stage 4 — Masked Self-Attention

"This is the heart of the decoder. Self-attention lets each token look at the other tokens to understand context. The word 'sat' needs to know it's connected to 'cat' and 'on.' [pause] The 'masked' part is important: when the model is generating text, it can only look at words that came before. It can't peek at future words — that would be cheating. So we mask out all future positions, forcing each word to only attend to the past."

---

## Slide 12 — Stage 5 — Multi-Head Attention

"Instead of doing attention once, the model does it multiple times in parallel — these are called 'heads.' [pause] Each head focuses on a different type of relationship. One head might look for subject-verb connections, another for grammar, another for meaning. [pause] Then all the heads' results are combined together. This is like having multiple experts each reading the sentence from a different angle, then pooling their findings."

---

## Slide 13 — Stage 6 — Feed-Forward Network

"After attention has mixed information between tokens, each token gets processed individually through a feed-forward network. [pause] Think of it as the model thinking deeply about each word on its own — expanding its representation, applying an activation, then shrinking it back down. [pause] This is where a lot of the actual computation and 'thinking' happens."

---

## Slide 14 — Stage 7 — Residual Connection

"Residual connections are simple but crucial. After attention or feed-forward processes the data, we add the original input back in. [pause] Why? Because it prevents the model from losing important information. It's like making a photocopy of your work before editing — you keep the original and add the changes on top. [pause] This also makes training much more stable."

---

## Slide 15 — Stage 8 — Layer Normalization

"Layer normalization scales all the values in the vector to a similar range. [pause] Without it, some numbers could become huge and others tiny, which breaks training. [pause] It's like adjusting the volume on different audio tracks so they're all at a similar level before mixing them together."

---

## Slide 16 — The Decoder Block

"Now we put it all together into one decoder block: Masked Self-Attention, then Add & Normalize, then Feed-Forward, then Add & Normalize again. [pause] The 'Add' steps are the residual connections, and the 'Normalize' steps are the layer normalization. [pause] This single block is the building block that gets repeated."

---

## Slide 17 — Stacking N Blocks

"One block does one round of processing. To get deeper understanding, we stack many of these blocks on top of each other — anywhere from 12 to 128 depending on the model. [pause] The output of block 1 feeds into block 2, which feeds into block 3, and so on. Each layer refines the representation a little more, building increasingly abstract understanding. It's like editing an essay through multiple drafts."

---

## Slide 18 — Decoder Block Flow

"This animation shows data flowing through a single decoder block from top to bottom. [pause] Input enters, goes through masked self-attention, gets normalized with the residual added back, passes through the feed-forward network, gets normalized again with another residual, and exits. [pause] Watch the particle flow through each step and light it up. This exact process happens N times, once per block."

---

## Slide 19 — Output Head

"After all the decoder blocks, we reach the output head. The final vector of the last token is projected onto the entire vocabulary — every word the model knows. [pause] Each word gets a score, then we convert those scores into probabilities. The word with the highest probability is chosen as the next word. [pause] So for input 'The cat sat on,' the model might assign 78% probability to 'mat' and pick it."

---

## Slide 20 — Autoregressive Loop

"But predicting one word isn't enough — we want full sentences. So we take the predicted word, append it to the input, and run the entire pipeline again from scratch. [pause] 'The cat sat on' → predict 'the' → 'The cat sat on the' → predict 'mat' → and so on. [pause] This loop repeats until the model produces a stop token or reaches a length limit. That's why it's called autoregressive — the model's own output becomes part of its next input."

---

## Slide 21 — Full Pipeline

"Putting it all together: text enters, gets tokenized, embedded, positionally encoded, flows through N decoder blocks of attention and feed-forward processing, and the output head predicts the next word. [pause] Then that word is fed back in and the process repeats. [pause] This is the complete mechanism behind every word ChatGPT generates."

---

## Slide 22 — Thanks

"Thank you. The key takeaways: the Transformer decoder uses attention to understand context, stacks many identical blocks to build deep understanding, and predicts one word at a time in a loop. [pause] Any questions?"

---

## Tips for delivery

- Speak slowly and pause at each [pause] marker
- Let animations play for 2–3 seconds before explaining
- On the 3D slides, give the audience a moment to look around
- If asked about QKV, explain it happens inside the Masked Self-Attention stage
- If asked why there's no encoder, explain GPT is decoder-only (dropped the encoder from the original 2017 paper)

---

# EXTRA Q&A — Common Questions & Answers

Use these if the audience asks. Each is written to be read aloud in simple terms.

---

## Q: What is the difference between an encoder and a decoder?

"The original 2017 Transformer had two parts: an encoder and a decoder. The encoder reads the input and builds an understanding of it. The decoder uses that understanding to generate output. [pause] For translation, the encoder reads English and the decoder writes French. [pause] GPT removed the encoder entirely and kept only the decoder. That's why GPT is called a 'decoder-only' model. It both reads the input and generates the output using just the decoder."

---

## Q: What are Query, Key, and Value (QKV)?

"These are three transformed versions of each token's vector, computed inside the attention step. [pause] Think of it like a library. The Query is 'what am I looking for?' The Key is 'what does this book contain?' The Value is 'the actual information in this book.' [pause] The model compares every query against every key to find matches, then pulls the values from the matching tokens. That's how attention decides which words are relevant."

---

## Q: What is layer normalization?

"Layer normalization is a technique that keeps the numbers in each vector from getting too big or too small. [pause] If some values become huge and others tiny, the model becomes unstable and can't learn properly. [pause] Normalization rescales everything to a consistent range, like turning the volume up or down on different audio tracks so they're all balanced before mixing."

---

## Q: What is a residual connection?

"A residual connection means we add the original input back to the output of a layer. [pause] After attention processes the data, instead of throwing away the original, we add it back. [pause] This does two things: it prevents the model from losing important information, and it makes training much more stable. It's like keeping a backup copy of your work before making edits."

---

## Q: What is softmax?

"Softmax is the function that turns raw scores into probabilities. [pause] After the model scores every word in the vocabulary, those scores could be any numbers — positive, negative, large, small. [pause] Softmax converts them into percentages that all add up to 100%, so the highest-scoring word becomes the most likely prediction."

---

## Q: What is an attention score?

"An attention score is how much one token should 'pay attention to' another token. [pause] It's calculated by comparing the Query of one token with the Key of another. High score = highly relevant. Low score = not relevant. [pause] These scores are then converted to probabilities with softmax and used to weight the Values, blending information from the most relevant tokens."

---

## Q: What is multi-head attention?

"Multi-head attention means the model runs attention several times in parallel, each with a different focus. [pause] One attention 'head' might track subject-verb relationships, another might track grammar, another might track meaning. [pause] All heads run simultaneously, then their results are combined. It's like having multiple experts each reading the sentence from a different angle."

---

## Q: What is a vocabulary?

"The vocabulary is the complete list of all tokens the model knows — every word, word piece, and punctuation mark it can recognize or generate. [pause] GPT-3 used a vocabulary of about 50,000 tokens. Anything not in the vocabulary gets broken down into smaller sub-word pieces that are."

---

## Q: What is tokenization and what are the different types?

"Tokenization is splitting text into tokens. There are different methods: [pause] Word-level splits on spaces, character-level splits into individual letters, and sub-word methods like BPE (Byte Pair Encoding) which is what GPT uses. [pause] BPE breaks rare words into common pieces — for example, 'unhappiness' might become 'un', 'happi', 'ness'. This lets the model handle any word even if it wasn't in the training data."

---

## Q: What is the difference between training and inference?

"Training is when the model learns — it reads huge amounts of text, makes predictions, and adjusts its internal weights when it gets them wrong. This takes weeks and massive computing power. [pause] Inference is when you actually use the trained model — like typing into ChatGPT. The weights are fixed; it just runs the forward pass to predict the next word. Inference is much faster and cheaper."

---

## Q: What is temperature?

"Temperature controls how random the model's output is. [pause] A low temperature (like 0.1) makes the model almost always pick the highest-probability word — very predictable and factual. [pause] A high temperature (like 1.0 or more) makes the model more creative and random, sometimes surprising. [pause] It's a knob you turn based on whether you want factual answers or creative writing."

---

## Q: What is top-k and top-p sampling?

"These are ways to add variety to the output while keeping it sensible. [pause] Top-k means the model only considers the top k most likely words (like the top 50) and picks randomly from those. [pause] Top-p (nucleus sampling) means the model picks from the smallest set of words whose probabilities add up to p (like 90%). [pause] Both prevent the model from choosing really unlikely words while still allowing some creativity."

---

## Q: What is an embedding dimension?

"The embedding dimension is how many numbers are in each token's vector. [pause] GPT-2 small uses 768 dimensions. GPT-3 uses 12,288. [pause] More dimensions means the model can capture more subtle aspects of meaning, but it also means more computation. It's one of the main factors in how 'large' a model is."

---

## Q: What makes a model 'large'? What are parameters?

"Parameters are the numbers the model learned during training — the weights in all the neural network layers. [pause] A model with more parameters can store more patterns and knowledge, which generally makes it smarter. [pause] GPT-3 has 175 billion parameters. GPT-4 is estimated to have over 1 trillion. More parameters = bigger model = usually better, but also slower and more expensive to run."

---

## Q: Why did Transformers replace RNNs (older models)?

"Older models like RNNs read text one word at a time, left to right. This caused two problems: [pause] First, they couldn't look at all words simultaneously, so they struggled with long-range relationships (like a pronoun referring to a noun 50 words earlier). [pause] Second, they couldn't be parallelized — each word had to wait for the previous one. [pause] Transformers fix both: attention sees everything at once, and the whole sequence is processed in parallel, making them much faster to train."

---

## Q: Does the model actually 'understand' language?

"This is a philosophical question. Technically, the model is just predicting the most likely next word based on patterns it learned from training data. [pause] It doesn't have beliefs or experiences. [pause] However, because it trained on so much human text, it captures statistical patterns of how language works and how ideas connect, which makes its output often feel like genuine understanding."

---

## Q: How are embeddings learned?

"Embeddings aren't hand-designed — they're learned during training. [pause] The model starts with random vectors for every word. As it trains on text and gets better at predicting the next word, it gradually adjusts each word's vector so that similar words end up close together. [pause] After billions of training examples, the vectors naturally organize into meaningful clusters."

---

## Q: What is a feed-forward network exactly?

"A feed-forward network is the simplest type of neural network — input goes in one end, passes through one or more layers, and output comes out the other. [pause] In the decoder, it has two layers: the first expands the vector to a larger size (often 4x), applies an activation function like ReLU, and the second shrinks it back down. [pause] This lets the model apply non-linear transformations to each token independently, deepening its representation."
