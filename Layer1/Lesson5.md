*** Parameters & Inference ***

Now we're going to answer two fundamental questions:

  Where does an LLM's learned knowledge/patterns actually live?

and

  What happens when I send a prompt to an LLM?

The answers are parameters and inference.

1. What are parameters?

A parameter is a numerical value inside a neural network that is learned during training.

A modern LLM can have billions of parameters.

You may hear:

7B parameters
13B parameters
70B parameters
405B parameters

The B means billion.

So:

7B = approximately 7 billion parameters
70B = approximately 70 billion parameters

These aren't simply "70 billion facts."

That's an important distinction.

2. Think of parameters as learned patterns

Imagine you're training a developer to recognize code.

You show them thousands of examples:

function examples
database queries
API requests
React components
algorithms
etc.

Over time, they develop an internal understanding of patterns.

LLMs do something fundamentally different and mathematical, but the analogy helps:

Training changes the model's parameters so that it becomes better at predicting language and other patterns.

The parameters are the numerical values that get adjusted during training.

3. Where is the model's "knowledge"?

A simplified mental model is:

Training data
     ↓
Training process
     ↓
Parameters are adjusted
     ↓
Trained model
     ↓
Parameters encode learned patterns

So when someone asks:

"Where does GPT know this?"

The answer isn't:

"There's a database inside it containing every answer."

Instead, the model's learned behavior is encoded across its parameters and architecture.

This is one reason an LLM isn't simply a traditional database.

4. Parameters aren't individual facts

Suppose a model knows:

"Paris is the capital of France."

Don't imagine:

parameter #1 = Paris
parameter #2 = France
parameter #3 = capital

It doesn't work like that.

The information is represented in distributed numerical patterns across many parameters.

So:

Billions of parameters
        ↓
Distributed learned patterns
        ↓
Model behavior

This is a much better mental model.

5. Training vs inference

This distinction is extremely important.

Training

The model learns.

Data
 ↓
Model
 ↓
Prediction
 ↓
Error
 ↓
Adjust parameters
 ↓
Repeat

The parameters are changed during training.

Inference

The trained model uses what it has learned to generate an output.

Prompt
 ↓
Trained model
 ↓
Prediction
 ↓
Output

During ordinary inference, the model's learned parameters aren't being updated just because you asked it a question.

6. An analogy

Imagine studying for an exam.

Training

You spend months learning:

Programming
Databases
Networking
Mathematics

Your knowledge changes.

Inference

Someone asks:

"What is a database index?"

You use what you already learned to answer.

You aren't normally rewriting your brain's knowledge after every question.

Similarly:

Training → parameters change

Inference → parameters are used

This distinction is essential.

7. What happens when you send a prompt?

Let's connect everything we've learned.

You send:

"What is a database index?"
Step 1 — Tokenization
Text
 ↓
Tokens
 ↓
Token IDs
Step 2 — Model processes them

The Transformer processes those token representations using its layers and attention mechanisms.

Token representations
        ↓
Transformer layers
        ↓
Attention
        ↓
Neural computations
Step 3 — Model produces probabilities

The model estimates probabilities for possible next tokens.

Conceptually:

"An"       → 35%
"A"        → 20%
"A database" → ...
"Database" → ...

The exact probabilities aren't important here.

The key idea is:

The model produces a probability distribution over possible next tokens.

Step 4 — A token is selected

One token is selected according to the generation process.

Then the model generates another.

And another.

Prompt
 ↓
Token
 ↓
Token
 ↓
Token
 ↓
Token
 ↓
...

That's inference.

8. LLMs are fundamentally next-token predictors

This is one of the most important ideas in understanding LLMs.

Suppose:

The capital of France is

The model predicts something like:

Paris → very high probability
London → lower
Berlin → lower
...

It generates:

Paris

Now the sequence becomes:

The capital of France is Paris

The model predicts the next token again.

This continues until the generation ends.

So:

LLM generation is fundamentally an iterative next-token prediction process.

9. Then why does it look like reasoning?

This is where things get interesting.

The model can generate sequences that look like:

Problem
 ↓
Step 1
 ↓
Step 2
 ↓
Step 3
 ↓
Answer

It can perform surprisingly sophisticated tasks.

But underneath, the model is still performing neural-network computations to predict subsequent tokens.

This doesn't mean its capabilities are "just autocomplete" in the everyday sense.

Modern models can perform complex transformations and multi-step tasks.

But the core generation mechanism remains next-token prediction.

10. Parameters and model size

You may naturally ask:

"Does more parameters always mean a better model?"

No.

Parameter count is one factor among many.

Performance also depends on:

training data
data quality
architecture
training methods
post-training/alignment
context capabilities
inference techniques
tools
evaluation methods

So:

70B parameters

doesn't automatically mean:

better than every 30B model

Don't use parameter count as a simple quality ranking.

11. Parameters vs context

This distinction is extremely important.

Parameters

The model's learned internal values.

Parameters
 ↓
Learned patterns
Context

The information currently provided to the model.

Context
 ↓
Current information

Think:

PARAMETERS
"What the trained model has learned"

CONTEXT
"What we're giving it right now"

This distinction will become extremely important when we get to RAG.

12. RAG becomes easier to understand now

Suppose your company's latest sales report wasn't included in the model's training data.

You ask:

"How much revenue did our company make last month?"

The model's parameters don't magically contain your company's latest private report.

So your application can retrieve the report and put the relevant information into the context:

User question
      ↓
Retrieve company data
      ↓
Put relevant data into context
      ↓
LLM
      ↓
Answer

This is one of the fundamental reasons RAG exists.

13. The complete picture

You've now learned enough pieces to see the whole pipeline:

                    USER
                     │
                     ↓
                "What is X?"
                     │
                     ↓
               TOKENIZATION
                     │
                     ↓
                 TOKEN IDs
                     │
                     ↓
        ┌────────────────────────┐
        │       TRANSFORMER      │
        │                        │
        │ Attention              │
        │ Neural network layers  │
        │ Learned parameters     │
        └───────────┬────────────┘
                    ↓
             Token probabilities
                    ↓
              Select token
                    ↓
              Select next token
                    ↓
                    ...
                    ↓
                  ANSWER

This is inference.

14. One subtle but important point

When you call an LLM API, you're generally not training the model yourself.

For example, your application might do:

Your application
      ↓
LLM API
      ↓
Provider's trained model
      ↓
Response

Your job as an AI engineer is often to build the software around the model:

prompts
context
tools
RAG
databases
permissions
evaluation
APIs
UI
monitoring
cost control

This is exactly where your existing software-engineering background becomes valuable.

🧠 Mental model to keep

Remember these four:

Parameters

Learned numerical values inside the model.

Training

The process that adjusts parameters so the model learns patterns.

Inference

Using the trained model to generate an output.

Next-token prediction

The fundamental mechanism through which an autoregressive LLM generates text.

And the distinction:

PARAMETERS = learned patterns

CONTEXT = information currently provided

INFERENCE = using the model to generate output