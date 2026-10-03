*** Model Limitations & Hallucinations ***

So far, we've learned what LLMs can do.

Now we need to understand what they cannot reliably do.

The core principle is:

  An LLM is a powerful generator of language and patterns, not a guaranteed source of truth.



  1. What is a hallucination?

An LLM hallucination is when a model generates information that is false, unsupported, or fabricated while presenting it as if it were a valid answer.

For example, imagine asking:

"Who wrote the book The Moon of Mogadishu?"

If that book doesn't exist, an LLM might nevertheless produce:

"It was written by Ahmed Hassan in 1987."

That answer may sound completely reasonable.

But it could be entirely fabricated.

That's a hallucination.

2. Why does this happen?

Remember our previous lesson:

LLMs generate tokens based on learned patterns and probabilities.

The model's fundamental task isn't:

"Determine whether this statement is objectively true."

Its fundamental generation process is closer to:

"Given everything in my context, what tokens are likely to come next?"

That's a huge distinction.

Consider:

"What is the capital of France?"

The model has strong learned patterns around:

France → capital → Paris

So it can produce:

Paris

But imagine asking:

"What was the exact revenue of Company X in Q3 2026?"

If that information isn't available in the model's knowledge or current context, the model doesn't necessarily have a reliable mechanism that forces it to say:

"I don't know."

It may generate a plausible-looking answer.

3. The dangerous part: plausible ≠ true

This is probably the most important sentence in this lesson:

An answer can sound extremely confident and still be wrong.

For example:

Question
   ↓
LLM
   ↓
Very fluent answer
   ↓
❌ Incorrect information

Fluency is not proof of correctness.

This is why you should never design an important AI system around:

"The model sounds confident, therefore it's correct."

4. Why hallucinations happen

There isn't one single cause.

Several factors can contribute.

1. Missing information

The model doesn't have the required information.

2. Outdated information

The model's training data may not contain recent events.

3. Ambiguous questions

The model has to interpret something unclear.

4. Weak or conflicting context

You give the model poor information.

5. Long/complex reasoning chains

The model can make mistakes while generating multiple steps.

6. Pressure to answer

If your application always expects an answer, the model may generate something rather than clearly saying it lacks sufficient information.

5. Knowledge cutoff

Another important limitation is that a trained model doesn't automatically know events that happened after its relevant training period.

Imagine:

Training data
     ↓
Model training
     ↓
Model released
     ↓
New event happens

The model's parameters don't automatically update because the event happened.

This is why modern AI applications often connect models to external sources.

For example:

LLM
 +
Web search
 +
Database
 +
Company documents
 +
APIs

Now the application can provide current information in the context.

6. Parameters vs current information

Let's connect this to our previous lesson.

Imagine your company's database contains:

Customer: Ahmed
Balance: $270

The LLM's parameters don't automatically contain that customer's current balance.

Instead:

Your database
     ↓
Application
     ↓
Relevant information
     ↓
LLM context
     ↓
Answer

This is a fundamental AI-engineering architecture.

7. RAG helps with this

Remember our earlier introduction to RAG?

Suppose you have:

Company documents

Instead of asking the model to rely entirely on what it learned during training:

Question
   ↓
LLM
   ↓
Answer

you can do:

Question
   ↓
Retrieve relevant documents
   ↓
Relevant information
   ↓
LLM context
   ↓
Answer

This gives the model external grounding.

It can substantially reduce certain types of hallucination, although it does not guarantee perfect accuracy.

We'll study RAG properly later.

8. Tool calling also helps

Suppose a customer asks:

"What is my current account balance?"

Don't ask the LLM to guess.

Give it a tool:

getCustomerBalance(customerId)

Then:

User
 ↓
LLM
 ↓
Tool call
 ↓
Database
 ↓
$270
 ↓
LLM
 ↓
Answer

Now the model doesn't have to invent the current balance.

This is one reason tool calling will be such an important topic later.

9. LLMs can also make reasoning mistakes

Suppose you ask:

If a shop has 37 products and sells 18,
then receives 9 more,
how many does it have?

The model may answer correctly:

28

But models can still make arithmetic or logical mistakes, particularly with complicated tasks.

That's why AI systems sometimes use tools such as:

Calculator
Python
Database
Search
Code execution

Instead of forcing the LLM to do everything itself.

A useful engineering principle is:

Use the LLM for what it's good at, and tools for tasks where deterministic computation or authoritative data is better.

10. LLMs don't automatically know your business rules

Imagine your shop software has this rule:

"A customer can rent an item only if they have one of three types of guarantees."

The LLM won't automatically know that.

You need to provide the rule through something like:

System instructions
+
Database
+
RAG
+
Tools

Then the model can operate within your application's actual rules.

This becomes extremely important when you build production systems.

11. More context doesn't automatically solve everything

You might think:

"I'll just give the model all the information."

Not necessarily.

Too much context can introduce:

irrelevant information
conflicting information
higher cost
higher latency
harder retrieval
context-management problems

So AI engineering isn't simply:

Give the model more information.

It's:

Give the model the right information at the right time.

12. Hallucinations are not simply a "bug"

This is an important mindset.

You shouldn't approach an LLM like:

Normal software:

Input → deterministic function → correct output

Instead:

LLM:

Input
 ↓
probabilistic neural computation
 ↓
generated output

The system is inherently probabilistic.

So production AI engineering requires:

validation
evaluation
grounding
tool use
monitoring
guardrails
good prompts
appropriate model selection
13. How AI engineers reduce hallucinations

You'll eventually learn several techniques.

Technique 1 — Better prompting

Tell the model:

"If the information isn't provided, say you don't have enough information."

Useful, but not sufficient by itself.

Technique 2 — RAG

Retrieve authoritative information and put it into context.

Technique 3 — Tool calling

Allow the model to retrieve live data or perform deterministic operations.

Technique 4 — Structured outputs

Force responses into a defined schema.

Technique 5 — Validation

Your application checks the model's output.

Technique 6 — Evaluation

Test the system systematically against known examples.

Technique 7 — Human approval

For high-risk operations, require a human to approve actions.

14. A production mindset

Imagine you're building an AI agent that can refund customers.

Bad architecture:

Customer
   ↓
LLM
   ↓
"Refund customer"

You're trusting the model too much.

A safer architecture might be:

Customer request
       ↓
      LLM
       ↓
Determine intended action
       ↓
Check permissions
       ↓
Check business rules
       ↓
Call refund tool
       ↓
Validate result
       ↓
Return response

The LLM isn't given unrestricted authority.

This distinction between LLM intelligence and software control will become very important when we study agents.

15. The biggest mental shift

Don't think:

"The AI knows everything."

Think:

"The model is a powerful reasoning and language component inside a larger software system."

That system can provide:

LLM
+
Database
+
RAG
+
APIs
+
Tools
+
Business rules
+
Validation
+
Permissions
+
Evaluation

That's what a real AI application looks like.

🧠 Your Layer 1 mental model so far

You've now learned seven pieces:

1. Tokens
      ↓
2. Context
      ↓
3. Transformer
      ↓
4. Attention
      ↓
5. Parameters + inference
      ↓
6. Temperature + top-p
      ↓
7. Limitations + hallucinations

And the most important overall picture:

                    USER
                      ↓
                    PROMPT
                      ↓
                  TOKENIZE
                      ↓
                   TOKENS
                      ↓
                TRANSFORMER
                 ↙         ↘
           PARAMETERS    ATTENTION
                 ↘         ↙
                  PROCESS
                     ↓
              PROBABILITIES
                     ↓
               SAMPLING
                     ↓
               NEXT TOKEN
                     ↓
                  ...
                     ↓
                  OUTPUT

But for a production AI application, we can add:

                  EXTERNAL DATA
                       ↓
                 RAG / TOOLS
                       ↓
USER → APPLICATION → LLM → VALIDATION → RESULT

That distinction—model vs application around the model—is central to becoming an AI Engineer.