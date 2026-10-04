*** Few-Shot Prompting ***

Now we move from "tell the model what to do" to a more powerful idea:

        Show the model what good output looks like.

This is called few-shot prompting.


1. What is few-shot prompting?

Few-shot prompting means giving the model a small number of examples of the task before asking it to perform the actual task.

Zero-shot

You give only the instruction:

Classify this message as Positive or Negative.

Message:
"The product arrived early and works perfectly."

The model has to infer what you mean by "Positive" and "Negative."

Few-shot

You provide examples first:

Classify the message as Positive or Negative.

Example 1:
Message: "The product is excellent."
Answer: Positive

Example 2:
Message: "The product stopped working."
Answer: Negative

Now classify:

Message: "The product arrived early and works perfectly."
Answer:

The examples teach the model the pattern you expect.

2. Why does this work?

Remember from Layer 1:

An LLM is fundamentally a pattern-learning system.

When you provide examples, you're giving the model additional information about the task.

Instead of:

Instruction → Answer

you're giving:

Instruction
     ↓
Example
     ↓
Example
     ↓
Example
     ↓
New input
     ↓
Expected pattern

The model can infer:

"Ah, this is how the developer wants me to perform this task."

3. Zero-shot vs few-shot

There are three useful terms.

Zero-shot

No examples.

Instruction → Task
One-shot

One example.

Instruction
↓
One example
↓
Task
Few-shot

A small number of examples.

Instruction
↓
Example 1
↓
Example 2
↓
Example 3
↓
Task

"Few" doesn't necessarily mean exactly 3. It simply means a small set of examples.

4. A software-engineering example

Imagine you're building an AI assistant for customer messages.

You want it to classify messages into:

billing
technical
sales
other

You could tell the model:

Classify this customer message into:
billing, technical, sales, or other.

But you can make the intended behavior clearer with examples:

Example:
"I was charged twice."
→ billing

Example:
"The application crashes when I log in."
→ technical

Example:
"How much does the enterprise plan cost?"
→ sales

Now classify:
"Why was my card charged this month?"

The examples establish the pattern.

5. Few-shot is especially useful for ambiguous tasks

Consider this:

"Can you check my account?"

What category is that?

Could be:

billing
technical
account support

The instruction alone may not fully define what you want.

Examples can clarify the intended classification behavior.

For example:

"I can't log into my account."
→ technical

"I want to change my account information."
→ account

"I was charged unexpectedly."
→ billing

Now the model has a better understanding of your application's interpretation.

6. Examples are not just demonstrations

This is an important AI engineering insight.

A good example can communicate things that are difficult to explain with words.

Suppose you want the model to extract information from a Somali customer message.

Instead of writing a huge instruction explaining every possible linguistic variation, you could provide examples:

Input:
"Axmed wuxuu leeyahay 50 dollar oo deyn ah."

Output:
{
  "customer": "Axmed",
  "amount": 50,
  "type": "debt"
}

Then:

Input:
"Fatima waxay iga qabtaa 20 dollar."

Output:
{
  "customer": "Fatima",
  "amount": 20,
  "type": "debt"
}

Now the model can infer the transformation you're looking for.

7. The quality of examples matters

This is where many people get it wrong.

If your examples are bad, the model can learn the wrong pattern.

Imagine:

Example 1:
Customer: "I hate this product."
Label: Positive

Then:

Customer: "This product is terrible."

The model may reasonably become confused.

Your examples should therefore be:

Correct
Relevant
Consistent
Representative
Clear

Think of them almost like test cases.

8. Few-shot prompting is similar to unit tests

As a developer, this analogy is useful.

Suppose you have a function:

classify(message)

You might have:

classify("I was charged twice")
→ billing

classify("The app crashes")
→ technical

classify("How much is the plan?")
→ sales

Those examples define expected behavior.

Few-shot prompts do something conceptually similar:

Input → Expected Output

But there is an important difference:

Unit tests enforce behavior deterministically.

Few-shot examples merely guide the model.

The model can still make mistakes.

9. More examples isn't automatically better

You might think:

"If 3 examples help, 100 examples must be better."

Not necessarily.

More examples consume context.

Remember our Layer 1 lesson:

Context is limited and has cost/latency implications.

If you put 500 examples into every request:

Huge prompt
   ↓
Higher token usage
   ↓
More latency
   ↓
Potentially less room for actual user context

You need a useful set of examples, not every example you've ever collected.

10. Choosing good examples

Suppose you're building a support classifier.

Don't choose three nearly identical examples:

"My app crashes."
"The app crashes."
"My application crashes."

They don't teach much additional behavior.

Instead, choose examples that cover different situations:

"My app crashes when I log in."
→ technical

"I was charged twice."
→ billing

"How much is the premium plan?"
→ sales

The examples cover different parts of the task.

This is called coverage.

11. Few-shot + structured output

These concepts can work together.

You might have:

Instruction:
Extract customer information.

Example:
Input:
"Ahmed owes me $50."

Output:
{
  "customer": "Ahmed",
  "amount": 50
}

Now process:
"Fatima owes $30."

And additionally require a schema:

customer: string
amount: number

Now you have:

Instructions
      +
Examples
      +
Output constraints
      ↓
LLM
      ↓
Structured result

This combination becomes very powerful in real applications.

12. Few-shot doesn't mean training the model

This distinction is extremely important.

Suppose you send:

Example 1
Example 2
Example 3

The model has not learned these examples permanently.

You're providing them inside the current context.

After the request is finished, the model's parameters haven't changed.

Remember:

Prompting changes the context. Training changes the parameters.

That's the connection to Layer 1.

Few-shot prompting
        ↓
Changes current context

Fine-tuning
        ↓
Changes model parameters

These are completely different mechanisms.

13. When should you use few-shot prompting?

It's particularly useful when:

The task is ambiguous
You need a specific style
You need consistent classification
The desired transformation is difficult to explain
You have unusual business rules
You need the model to follow a particular format
Zero-shot performance isn't reliable enough

But don't automatically use few-shot for every task.

Start simple:

Instruction
   ↓
Test
   ↓
If insufficient
   ↓
Add examples
   ↓
Evaluate again

That's the engineering approach.

14. The key AI-engineering mindset

Don't ask:

"How many examples should I put in my prompt?"

Ask:

"What behavior does the model currently get wrong, and can a carefully selected example clarify that behavior?"

That's much more useful.

Your examples should solve specific failure modes.

Mental model

Remember this:

ZERO-SHOT

"Do this."
     ↓
   Model
     ↓
  Output


FEW-SHOT

"Do this."

Example → Expected output
Example → Expected output
Example → Expected output

     ↓
   Model
     ↓
  Output

And the most important sentence:

Few-shot prompting teaches the model the desired pattern through examples, without changing the model's parameters.

Layer 2 progress
✅ System vs user instructions
✅ Few-shot prompting
⏳ Structured prompting
⏳ Output constraints
⏳ Prompt decomposition
⏳ Reasoning/task planning patterns
⏳ Prompt injection & instruction conflicts
⏳ Prompt evaluation