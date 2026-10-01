***Context Windows***


  - How much information can an LLM work with at one time?


1. What is a context window?

A context window is the amount of token-based information a model can consider in a single request/conversation context.

Think of it as the model's working area.

For example, imagine a model has:

   Context window = 128,000 tokens

The model can process up to that model-specific limit of context in a request.

The context can include things such as:

   System instructions
         +
   User messages
         +
   Previous conversation
         +
   Retrieved documents
         +
   Tool results
         +
   Other input

Conceptually:

┌──────────────────────────────────────┐
│          CONTEXT WINDOW              │
│                                      │
│ System instructions                  │
│ Conversation history                 │
│ User's current question              │
│ Retrieved information                │
│ Tool results                         │
│                                      │
└──────────────────────────────────────┘
                    ↓
                  LLM
                    ↓
                 Output
2. Think of it as the model's desk

Imagine you're a developer working at a desk.

Your desk can hold:

   your laptop
   documentation
   database schema
   notes
   requirements
   code

But eventually the desk becomes full.

You can't keep putting more papers on it.

A context window is similar.

The model has a finite amount of information it can process in that context.

So:

   Context window ≈ the model's available working space for tokens.

It's not literally memory or physical storage, but this is a useful mental model.

3. Context window ≠ memory

This distinction is very important.

Suppose you have a conversation:

   User: My name is Ahmed.

   ...

   User: What is my name?

If the earlier message is included in the model's current context, the model can use it.

But that doesn't necessarily mean the model has permanently stored your name.

Think:

   Context
   = information currently supplied to the model

   Memory
   = information retained outside that immediate context

We'll encounter different kinds of memory later when studying AI agents.

4. Why does context matter?

Imagine you build a customer-support AI.

A customer sends:

   "What is the status of my order?"

Your system might provide the model with:

   System instructions
   +
   Customer information
   +
   Order information
   +
   Relevant company policy
   +
   Conversation history
   +
   Current question

All of that consumes context.

If you unnecessarily send:

10,000 old conversations
+
entire company database
+
all company documentation
+
every product description

you're wasting context.

This leads to a major AI-engineering principle:

Give the model the information it needs, not everything you have.

This becomes extremely important when we reach RAG.

5. Context windows have grown dramatically

Older language models had relatively small context windows.

Modern models can support very large contexts, sometimes hundreds of thousands or more tokens depending on the model.

But:

Bigger context does not mean you should dump everything into the model.

Why?

Because context has costs and engineering tradeoffs:

   more tokens
   potentially higher cost
   potentially higher latency
   more irrelevant information
   more opportunities for conflicting information
   potentially worse retrieval/use of relevant information

So even if you can send 100,000 tokens, that doesn't mean you should.

6. Context and RAG

This is where your future AI-engineering knowledge starts connecting.

Suppose you have:

1,000 company documents

You could theoretically send huge amounts of them to an LLM.

But that's inefficient.

Instead:

User question
      ↓
Search relevant information
      ↓
Retrieve 5 useful chunks
      ↓
Put those chunks into context
      ↓
LLM
      ↓
Answer

This is the basic idea behind Retrieval-Augmented Generation (RAG).

We'll study it later.

For now, remember:

RAG helps control what information enters the model's context.

7. Context is also important for agents

Imagine an AI agent doing this:

User request
   ↓
Agent
   ↓
Tool call
   ↓
Tool result
   ↓
Agent thinks/acts
   ↓
Another tool
   ↓
Another result
   ↓
Agent

Every step can potentially add information to the context.

For example:

User request                 500 tokens
System instructions        2,000 tokens
Tool result #1             1,500 tokens
Tool result #2             4,000 tokens
Conversation history       3,000 tokens
...

The context can grow quickly.

That's why AI engineers need to think about:

summarization
pruning old messages
retrieving only relevant information
limiting tool output
structured state
context management
8. Context window and output

One subtle point:

The model needs room for its output as well, depending on the API/model's specific limits.

Conceptually:

┌──────────────────────────────────────┐
│           Available context          │
│                                      │
│ Input/context                        │
│ ████████████████████████             │
│                                      │
│ Space for generated output           │
│ ████████                             │
└──────────────────────────────────────┘

So you shouldn't think:

"The model has 128k tokens, therefore I can always send 128k tokens and get a huge response."

The exact accounting depends on the model/API, but the general engineering principle is:

Input/context and generated output both have limits.

9. Context window vs token limit

These terms are related but shouldn't be treated as identical.

Token limit can refer to different constraints, such as:

maximum input tokens
maximum output tokens
maximum total context

The exact limits depend on the model/API.

As an AI engineer, always check the specific model's documentation instead of assuming every model has the same limits.

10. A practical example

Imagine you're building an AI assistant for a university.

The university has:

5,000 documents

A student asks:

"What documents do I need to graduate?"

Bad architecture:

5,000 documents
        ↓
       LLM
        ↓
     Answer

You're giving the model enormous amounts of potentially irrelevant information.

Better architecture:

Student question
       ↓
Search
       ↓
Relevant graduation documents
       ↓
Relevant chunks
       ↓
Context
       ↓
LLM
       ↓
Grounded answer

This is the architectural thinking we'll develop throughout your AI Engineer journey.

🧠 The mental model to keep

Remember these three concepts:

Tokens

The pieces of text the model processes.

Context window

The amount of token-based information the model can work with in a particular context.

Context management

Deciding what information should actually be given to the model.

That third one will become a major AI-engineering skill.