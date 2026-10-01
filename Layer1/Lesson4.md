*** Attention ***

This is one of the most important concepts in your entire LLM foundation.

If you understand attention well, Transformers become much easier to understand.

1. The problem attention solves

Consider this sentence:

"The developer opened the laptop because it was broken."

What does "it" refer to?

The model needs to determine that:

    it → laptop

     rather than:

    it → developer

To do that, the model needs to look at relationships between different tokens.

That's the basic idea behind attention:

Attention allows the model to determine which tokens are relevant to each other when processing a sequence.

2. Think about how you read

When you read:

"Ahmed went to the bank because he needed money."

Your brain doesn't treat every word as equally important for understanding every other word.

When interpreting:

"he"

you naturally connect it strongly with:

"Ahmed"

Attention gives neural networks a mechanism for doing something somewhat analogous:

Ahmed ───────────────→ he
       strong relationship

Again, this is only an analogy. The model isn't literally thinking like a human.

3. Attention looks at relationships

Consider:

The cat sat on the mat because it was tired.

When processing:

"it"

the model may assign different attention weights to surrounding tokens.

Conceptually:

    The      → low
    cat      → high
    sat      → medium
    on       → low
    the      → low
    mat      → high
    because  → medium
    it       → current token
    was      → medium
    tired    → high

These aren't actual values; they're just illustrating the idea.

The model calculates numerical relationships rather than simply treating every token independently.

4. Attention weights

This is where the word attention comes from.

The model calculates something like:

"How much should this token pay attention to the other tokens?"

Conceptually:

          Token relationships

Ahmed  ───────────────→ he
        0.85

developer ────────────→ code
            0.72

France ───────────────→ Paris
         0.94

The actual system uses mathematical calculations to produce these relationships.

You don't need the equations yet.

The key concept is:

Attention produces weights that determine how strongly information from different tokens contributes to the representation being computed.

5. Why is this powerful?

Because language is full of relationships.

For example:

"The phone that Ahmed bought yesterday stopped working because it was damaged."

The model needs to deal with relationships between:

phone
  ↓
it

and:

Ahmed
  ↓
bought

and:

phone
  ↓
damaged

These relationships aren't always next to each other.

Attention lets the model consider relationships across the sequence.

6. Attention isn't simply "look at nearby words"

This is important.

Suppose:

"The software engineer who moved from London to Nairobi last year finally launched the application."

The relationship between:

engineer

and:

launched

is separated by many tokens.

Attention can allow the model to connect information across those positions.

That's one reason Transformers became so powerful for language.

7. Self-attention

You will hear this term constantly.

Self-attention means that the tokens in a sequence attend to other tokens within the same sequence.

For example:

The developer wrote the code

The token:

        developer

        can attend to:

        The
        wrote
        code

        And:

        code

        can attend to:

        developer
        wrote

So the sequence isn't processed as completely isolated tokens.

Instead, the model builds contextual representations.

8. Why context changes the meaning

Consider the word:

bank

What does it mean?

Sentence A

"I deposited money at the bank."

Here:

bank → financial institution
Sentence B

"We sat beside the river bank."

Here:

bank → side of a river

The token itself is the same:

bank

But the surrounding context changes its meaning.

Attention helps the model incorporate surrounding tokens when constructing its representation.

That's why context is so important.

9. A simplified example

Imagine:

The dog chased the ball because it was excited.

When processing:

it

the model can consider:

The
dog
chased
the
ball
because
it
was
excited

Attention helps determine which pieces of this information are relevant.

Conceptually:

                    ┌── dog ────────┐
                    │               │
The → dog → chased → ball → because → it
                    │               │
                    └───────────────┘

The actual architecture is much more mathematical, but this is the right intuition.

10. Attention doesn't mean the model "understands"

Be careful with this.

We often casually say:

"The model understands that 'it' refers to the dog."

Technically, that's shorthand.

The model is performing numerical transformations that produce useful representations and predictions.

A better technical mental model is:

Attention allows information from different tokens to influence each other's representations according to learned relationships.

That sentence is worth remembering.

11. Query, Key, Value — the next level

Eventually you'll hear:

Query
Key
Value

These are fundamental to understanding the mathematics of attention.

Very simplified:

Query

What information am I looking for?

Key

What kind of information does this token contain?

Value

What information should I actually take from this token?

Imagine you're searching a database.

Query
   ↓
"What information am I looking for?"

Keys
   ↓
"Which pieces might match?"

Values
   ↓
"Give me the actual information."

The attention mechanism compares queries with keys, calculates relevance scores, and uses those scores to combine values.

Conceptually:

Query
  ↓
Compare with Keys
  ↓
Attention scores
  ↓
Weight Values
  ↓
New representation

We'll revisit this when you're ready for the deeper technical layer.

For now, don't memorize the equations.


12. Why attention matters for AI engineering

You're going to encounter attention everywhere.

It helps explain:

Why Transformers work

Attention is a central mechanism inside Transformer architectures.

Why context matters

Attention operates over information available in the model's context.

Why long inputs can be computationally expensive

Attention involves relationships among tokens, creating important computational considerations as sequence length grows.

Why context can influence meaning

A token's representation can incorporate information from other tokens.

Why LLMs can handle complicated language

Attention helps the model represent relationships across sequences.


13. Your mental model so far

We've now built:
                   TEXT
                  ↓
             TOKENIZATION
                  ↓
                TOKENS
                  ↓
             TOKEN IDs
                  ↓
             TRANSFORMER
                  ↓
          ┌───────────────┐
          │   ATTENTION   │
          │               │
          │ Which tokens  │
          │ are relevant  │
          │ to each other?│
          └───────┬───────┘
                  ↓
        Contextual representations
                  ↓
          Neural computation
                  ↓
        Next-token probabilities
                  ↓
            Next token

And this leads naturally to our next topic.

🧠 Quick checkpoint

Try answering these in your own words:

1. What problem does attention solve?
2. What is self-attention?
3. Why can the word "bank" have different meanings depending on the surrounding text?
4. What does an attention weight represent conceptually?
5. What are Query, Key, and Value supposed to represent?