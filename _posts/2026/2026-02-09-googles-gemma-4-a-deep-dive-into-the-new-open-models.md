---
layout: post
title: "Google's Gemma 4: A Deep Dive into the New Open Models"
date: 2026-02-09
video_url: https://www.youtube.com/watch?v=LTYF2UNXG2M
description: "An in-depth look at Google's new Gemma 4 models, their Apache 2.0 license, unique architecture, and a hands-on test of its coding capabilities."
categories: [AI Development & Integration, New in the Dev World]
tags: [gemma 4, google, open source, ai models, mixture of experts, llm]
---

Google just dropped Gemma 4, and there's a lot to unpack. The release includes four new models, a major license change, and some genuinely interesting architectural decisions, all with day-one support for running locally.

Let's break it all down.

### The Apache 2.0 License: A Game Changer

Perhaps the biggest news, even more significant than the models themselves, is the licensing. Gemma 4 is being released under an Apache 2.0 license. This is a huge deal for the open-source and local LLM communities.

Its predecessor, Gemma 3, shipped with a custom Google license that imposed restrictions on usage, redistribution, and commercial deployment. Consequently, many developers simply skipped it.

With Gemma 4, Google is adopting the same permissive license used by giants like Qwen, Mistral, and Llama. This means you can use it, modify it, sell products built on it, and deploy it however you want. No strings attached. No weird clauses. Google is finally playing in the same sandbox as everyone else in open-source AI, and that matters far more than any benchmark number.

### The Gemma 4 Model Family

So, what exactly did Google release? The family consists of four distinct models:

*   **Two Edge Models:** These are the smallest of the bunch, designed specifically for phones and other small devices. They are tiny but pack some clever tricks, including being the only models in the family that support audio input natively. Think on-device voice assistants and speech recognition.
*   **Gemma 4 26B-MoE:** This is a 26 billion parameter Mixture-of-Experts (MoE) model. While it has 25.2 billion total parameters, only 3.8 billion are active at any given time. This allows it to run at the speed of a ~4 billion parameter model while delivering quality closer to a 30 billion parameter model. It supports text, images, and even video input. This is the model we'll be testing later in this article.
*   **Gemma 4 31B (Dense):** This is the flagship, the raw power option. All 30.7 billion parameters fire on every single token, making it the highest quality model in the family. It's the ideal choice for a fine-tuning base and is already making waves, currently ranked #3 on the LMSys Arena open model leaderboard.

### A Closer Look at the 26B-MoE Architecture

Let's zoom in on the 26B-MoE model, as it represents a fascinating piece of engineering.

It features 128 "experts." For each token processed, a router selects the best eight experts to use, plus one shared expert that is always active. This means only nine experts (out of 128) are engaged at any moment, roughly 7% of the model's total capacity.

A deep dive into the model files, not just the press releases, reveals some impressive specifications:
*   **30 layers**
*   **16 attention heads**
*   **A massive vocabulary of 262,000 tokens.** For comparison, most models operate with a vocabulary between 32,000 and 150,000 tokens. A larger vocabulary allows the model to represent more words and symbols as single tokens, significantly boosting efficiency.
*   **A 256,000 token context window.** That's roughly the equivalent of a 500-page book.

Each expert's feed-forward network has a hidden dimension of just 704. These are not large, general-purpose networks but tiny, highly specialized ones. This is where the architectural novelty truly begins.

### Architecturally Novel: What Makes Gemma 4 Stand Out

Gemma 4 introduces three architectural choices that are a departure from other open models.

#### 1. Hybrid Sliding and Global Attention

Instead of every layer looking at the full context—a computationally expensive process—Gemma 4 alternates its approach. Five consecutive layers use **sliding window attention**, which only considers the nearest 1,024 tokens. Then, the sixth layer gets **full global attention**, allowing it to see the entire context.

This pattern repeats through all 30 layers, with the final layer always being global. The idea is simple but effective: most of the time, understanding a word only requires its immediate context. You don't need to scan a 200-page document from the beginning for every single word. By reserving global context for every sixth layer, the model gets a chance to look at the whole picture periodically. Local context is cheap; global context is saved for when it truly matters.

#### 2. Layer-Dependent KV Heads

The number of Key-Value (KV) heads changes depending on the layer type:
*   **Sliding window layers** use 8 KV heads with 256-dimensional keys and values.
*   **Global layers** use only 2 KV heads, but with larger 512-dimensional keys and values.

Fewer heads on the global layers mean the KV cache required for long-range context uses significantly less memory. This is a key factor in making the massive 256,000-token context window practical.

#### 3. Dual RoPE Frequencies

The model uses two different Rotary Position Embedding (RoPE) frequencies:
*   **Sliding window layers** use a standard frequency of 10,000.
*   **Global layers** use a frequency of 1,000,000.

This 100x higher frequency for global layers allows the position coding to scale to much longer sequences without losing resolution, a critical component for handling its extensive context window.

### Gemma 4 vs. Qwen 2-MoE

How does Gemma 4 stack up against another popular MoE model, Qwen 2?

| Feature | Gemma 4 26B-MoE | Qwen 2-MoE |
| :--- | :--- | :--- |
| **Experts** | 128 total | 128 total |
| **Active Experts** | 8 + 1 shared | 8 |
| **Context Window**| 256,000 tokens | 32,000 tokens |
| **Modality** | Text, Image, Video | Text only |

The most significant advantages for Gemma 4 are its vastly larger context window and its native multimodal support.

### Benchmark Performance

On benchmarks for math, reasoning, science, and coding, the 26B-MoE model scored within 1-3% of the full 31B dense model. It achieves this while using only about 1/8th of the compute per token—an incredibly efficient trade-off. The 31B model is ranked #3 among open models on the LMSys Arena, with the 26B-MoE close behind at #6.

### Hands-On: Running Gemma 4 Locally

Theory is great, but how does it perform in practice? We downloaded the 26B-MoE model from Hugging Face and ran it on a machine with an RTX 3060 GPU (12GB VRAM).

#### Test 1: General Q&A with `llama.cpp`

We started with a simple chat session to get a feel for its reasoning and response generation. We asked it to explain the meaning and importance of an Apache 2.0 license.

The model generated a comprehensive and accurate answer, breaking down the key permissions, conditions, and limitations. It correctly identified why developers and companies favor the license, citing legal certainty and protection from patent litigation.

Even with some CPU offloading (the model is ~16GB, exceeding the 12GB VRAM), it performed admirably, generating text at nearly 13 tokens per second. The response was fast, detailed, and well-structured. For general knowledge tasks, it's impressively capable running on local hardware.

#### Test 2: Coding with Open Code

Next, we tested its coding abilities. Using Open Code pointed to a local server running the model, we gave it a simple prompt: "Can you build me a blackjack game?"

The model responded by asking for a preferred language. We let it choose, and it selected Python.

It swiftly generated the Python code for the game logic, followed by an HTML file complete with CSS for styling and JavaScript for the front-end interactivity.

Here is a sample of the code it produced:

**`blackjack.py` (Simplified Logic)**
```python
import random

class Deck:
    def __init__(self):
        self.cards = []
        self.build()

    def build(self):
        self.cards = []
        ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
        suits = ['♠', '♣', '♥', '♦']
        for suit in suits:
            for rank in ranks:
                self.cards.append((rank, suit))

    def shuffle(self):
        random.shuffle(self.cards)

    def deal(self):
        if len(self.cards) > 0:
            return self.cards.pop()
        return None

class Player:
    def __init__(self, name):
        self.name = name
        self.hand = []
        self.score = 0

    def hit(self, deck):
        card = deck.deal()
        if card:
            self.hand.append(card)
            self.calculate_score()
        return card

    def calculate_score(self):
        self.score = 0
        aces = 0
        for rank, suit in self.hand:
            if rank.isdigit():
                self.score += int(rank)
            elif rank in ['J', 'Q', 'K']:
                self.score += 10
            elif rank == 'A':
                aces += 1
                self.score += 11
        while self.score > 21 and aces > 0:
            self.score -= 10
            aces -= 1

# Main game logic would go here...
```

**`index.html`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Blackjack</title>
    <style>
        body { font-family: sans-serif; text-align: center; }
        .hand { margin: 20px; }
        .card { display: inline-block; border: 1px solid black; border-radius: 5px; padding: 10px 20px; margin: 5px; }
        #controls button { padding: 10px 20px; font-size: 16px; }
    </style>
</head>
<body>
    <h1>Blackjack</h1>
    <div id="dealer-hand" class="hand">
        <h2>Dealer's Hand</h2>
    </div>
    <div id="player-hand" class="hand">
        <h2>Your Hand</h2>
    </div>
    <div id="controls">
        <button id="hit-btn">Hit</button>
        <button id="stand-btn">Stand</button>
    </div>
    <h2 id="result"></h2>

    <script>
        // Simplified JavaScript game logic would be here
        // It would handle dealing cards, player actions (hit/stand),
        // and determining the winner.
        document.getElementById('hit-btn').addEventListener('click', () => {
            // Logic to draw a card
            console.log("Player hits.");
        });

        document.getElementById('stand-btn').addEventListener('click', () => {
            // Logic for the dealer's turn
            console.log("Player stands.");
        });
    </script>
</body>
</html>
```

The result was a simple but fully functional blackjack game running in the browser. The logic was correct, and it was built entirely by the AI running on a local machine. This demonstrates its solid capability as a coding agent, even on a challenging, multi-file task.

### Final Thoughts

Gemma 4 is a truly interesting and powerful release. The combination of a permissive Apache 2.0 license, innovative architecture, and strong performance makes it a compelling option for developers and researchers. The MoE model's efficiency is particularly noteworthy, offering near top-tier performance on consumer-grade hardware.

The open-source community is undoubtedly already hard at work customizing and fine-tuning these models, and it will be exciting to see the powerful applications that emerge.
