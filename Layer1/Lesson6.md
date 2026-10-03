*** Temperature & Top-p ***


We now have an important piece from the previous lesson:

An LLM produces a probability distribution over possible next tokens.

For example, imagine the model sees:

"The capital of France is"

It might internally produce something conceptually like:
| Possible next token | Probability |
| ------------------- | ----------: |
| Paris               |        0.92 |
| London              |        0.02 |
| Berlin              |        0.01 |
| Rome                |        0.01 |
| ...                 |         ... |


The actual probabilities are model-dependent; this is just an illustration.

So the question becomes:

  How do we control the model's selection from those probabilities?

That's where temperature and top-p come in.


1. First: probability is not the same as certainty

Suppose the model predicts:

    Paris → 92%
    London → 2%
    Berlin → 1%
    Rome → 1%
    Other → 4%

The model isn't necessarily saying:

    "I know Paris with 92% certainty."

It's producing a probability distribution that is used by the generation process.

This distinction matters because LLMs generate text probabilistically.

2. Temperature

Temperature controls how much the probability distribution is flattened or sharpened during sampling.

The easiest mental model:

    Lower temperature → more predictable

    Higher temperature → more varied

Imagine:

    Temperature = low

The model strongly favors high-probability tokens.

    Paris → ████████████████████
    London → █
    Berlin → █
    Rome → █

At a higher temperature:

    Temperature = higher

the probabilities become more spread out:

    Paris → ████████████
    London → ███
    Berlin → ██
    Rome → ██
    ...

This makes lower-probability choices more likely.

3. A simple analogy: rolling a weighted die

Imagine a strange six-sided die:

    Paris    → 90%
    London   → 4%
    Berlin   → 2%
    Rome     → 2%
    Madrid   → 1%
    Other    → 1%
Low temperature

You're making the die more heavily biased toward Paris.

The result will usually be:

    Paris
    Paris
    Paris
    Paris
    ...
    Higher temperature

You're making the distribution more varied.

You might occasionally get:

    London
    Berlin
    Paris
    Paris
    Rome
    ...

Again, this is an analogy—not literally how temperature works mechanically.

4. Temperature doesn't add knowledge

This is very important.

Suppose the model doesn't know the answer.

Increasing temperature does not make it smarter.

It can actually make its output more unpredictable.

So:

    Temperature ↑

    doesn't mean:

    Intelligence ↑

Instead:

Temperature ↑
    → more randomness/variation in sampling
5. Low temperature isn't automatically "better"

It depends on the task.

Imagine you're generating:

    Database SQL

You generally want:

    consistent
    precise
    predictable

A lower temperature may be appropriate.

But imagine:

    Story ideas

You might want:

    variety
    creative alternatives
    unexpected combinations

A somewhat higher temperature can be useful.

So temperature is a generation control, not a quality slider.

6. Example

Suppose you ask:

    "Give me a name for a Somali technology company."

At lower temperature, you might get outputs that are more predictable:

    Hikma Technologies
    SomTech
    Somali Digital Solutions

At a higher temperature, the model may explore more unusual combinations.

The important thing isn't that one is objectively better.

The desired behavior depends on your application.

7. Top-p

Now we have another parameter:

    top-p, also called nucleus sampling.

Instead of saying:

    "Consider every possible token."

top-p says roughly:

    Only consider the smallest group of highest-probability tokens whose combined probability reaches a chosen threshold.

For example, suppose:

    Paris     60%
    London    15%
    Berlin    10%
    Rome       5%
    Madrid     3%
    Other      7%

If:

    top-p = 0.90

the system could include tokens until their cumulative probability reaches about 90%:

    Paris     60%
    London    15%
    Berlin    10%
    Rome       5%
    ----------------
    Total     90%

The lower-probability candidates are excluded from that sampling step.

8. Why is it called "nucleus"?

    Because the model selects a probability mass nucleus.

Instead of choosing based on a fixed number of tokens:

    Top 5 tokens

it chooses based on probability mass:

    Top tokens whose combined probability ≈ chosen p

That's the key difference.

9. Top-k vs top-p

You may encounter top-k too.

    Top-k

Choose the top K tokens.

Example:

    top-k = 5

Only the five highest-probability tokens are considered.

    Top-p

Choose tokens until their cumulative probability reaches p.

Example:

    top-p = 0.90

The number of tokens can vary depending on the probability distribution.

So:

    top-k → fixed number of candidates

    top-p → variable number based on probability mass

You don't need to master top-k right now, but knowing the distinction is useful.

10. Temperature and top-p are different

This is a common beginner mistake.

    They're not two names for the same thing.

Temperature

    Changes the shape of the probability distribution.

Top-p

    Limits the candidate pool based on cumulative probability.

Conceptually:

    Model
    ↓
    Probability distribution
    ↓
    Temperature changes distribution
    ↓
    Top-p limits candidates
    ↓
    Sampling
    ↓
    Next token

The exact implementation/order can depend on the generation system, so treat this as a conceptual model.

11. Should you use both?

You can, depending on the API/model.

But as a beginner, don't obsess over tuning both simultaneously.

A useful engineering approach is:

    Start with the model's defaults → test your application → adjust one generation control at a time → evaluate the results.

Don't assume:

    temperature = 0

means:

    "The model is guaranteed to be perfectly deterministic."

The exact behavior depends on the model and API implementation.

12. What should YOU remember as an AI engineer?

Think about these use cases:

Structured business extraction
Temperature: low

You want predictable outputs.

Example:

{
  "customer": "Ahmed",
  "amount": 120,
  "status": "unpaid"
}
Creative writing
Temperature: somewhat higher

You want more variation.

Classification
Temperature: low

You generally don't need creative randomness.

Brainstorming
Temperature: higher

Variation can be useful.

13. One subtle point: temperature doesn't fix hallucinations

Suppose your model invents:

"The company was founded in 1987."

Changing temperature doesn't magically make the model verify that claim.

Hallucination is fundamentally a different problem.

Later we'll study:

grounding
RAG
tool calling
evaluation
structured outputs

These are much more relevant to controlling factual reliability.

14. Connect this to what we've learned

Let's connect the entire chain:

    User prompt
        ↓
    Tokenization
        ↓
    Token IDs
        ↓
    Transformer
        ↓
    Attention + learned parameters
        ↓
    Probability distribution
        ↓
    Temperature / sampling controls
        ↓
    Select next token
        ↓
    Add token to context
        ↓
    Predict next token
        ↓
    Repeat
        ↓
    Final response

You're now getting very close to understanding the fundamental generation loop of an LLM.

🧠 The mental model

Remember:

Temperature

    Controls how concentrated or spread out the probability distribution is during sampling.

Lower → more predictable

Higher → more varied

Top-p

    Restricts sampling to a set of high-probability tokens whose cumulative probability reaches a chosen threshold.

Most important

Neither temperature nor top-p gives the model new knowledge.

They influence how it chooses what to generate, not what it knows.

Quick checkpoint

Make sure you can explain:

What is temperature?
What happens when temperature increases?
What is top-p?
What's the difference between top-k and top-p?
Why doesn't increasing temperature make an LLM smarter?
Why might you use lower randomness for structured business data but higher randomness for brainstorming?