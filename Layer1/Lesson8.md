*** Structured Outputs / JSON ***


So far, we've mostly asked the model to produce:

human-readable text

Now we want:

  machine-readable data that our application can use.



1. The problem with normal LLM responses

Imagine your backend asks:

"Extract the customer's name and debt amount from this message."

The model might respond:

The customer's name is Ahmed and he owes $270.

That's understandable to a human.

But your TypeScript application wants something predictable:

{
  "customerName": "Ahmed",
  "amount": 270
}

Now your code can do:

response.customerName
response.amount

That's the basic reason structured outputs matter.

2. What is structured output?

Structured output means asking the model to produce data that follows a predefined structure or schema.

Instead of:

Ahmed owes $270.

you want:

{
  "customerName": "Ahmed",
  "amount": 270,
  "currency": "USD"
}

The structure tells your application:

customerName → string
amount       → number
currency     → string
3. Why JSON?

JSON is extremely common because software can easily parse it.

For example:

{
  "name": "Ahmed",
  "age": 26,
  "isDeveloper": true
}

Your backend can parse that into an object.

In JavaScript/TypeScript:

const data = JSON.parse(response);

Then:

console.log(data.name);
console.log(data.age);

So the LLM becomes one component in your application.

4. Think about the difference
Normal LLM
User
 ↓
LLM
 ↓
Natural language
 ↓
Human reads it
Structured LLM
User
 ↓
LLM
 ↓
Structured data
 ↓
Application
 ↓
Code processes it

This is a major transition in AI engineering.

5. Example: extracting an invoice

Suppose you give the model:

Invoice from Hikma Store.

Customer: Ahmed
Total: $270
Status: unpaid

You want:

{
  "customer": "Ahmed",
  "total": 270,
  "status": "unpaid"
}

Your application can then:

LLM
 ↓
JSON
 ↓
Validate JSON
 ↓
Save to PostgreSQL

Now the AI isn't the entire application.

It's a component that converts unstructured information into structured information.

That's a very common real-world AI use case.

6. Schema

A schema describes what the output should look like.

For example:

customerName → string
amount       → number
status       → "paid" | "unpaid"

Conceptually:

{
  "customerName": "string",
  "amount": "number",
  "status": "paid | unpaid"
}

A real schema can be much more precise.

For example, with TypeScript + Zod:

const DebtSchema = z.object({
  customerName: z.string(),
  amount: z.number(),
  status: z.enum(["paid", "unpaid"]),
});

Now your application can validate the model's output.

7. Prompting for JSON vs true structured output

This distinction is very important.

You can simply tell the model:

"Return JSON only."

For example:

Return the customer's information as JSON.

The model might produce:

{
  "name": "Ahmed",
  "amount": 270
}

But merely asking for JSON doesn't necessarily guarantee that the output will always conform perfectly to your expected structure.

The model is still generating tokens.

So:

"Please return JSON" is not the same thing as guaranteed schema-constrained output.

Modern LLM APIs can provide structured-output/schema mechanisms that constrain the response much more reliably.

The exact capabilities depend on the provider and model.

8. Why validation is still important

Even if you use structured outputs, your application should still treat model output as untrusted input.

Think about a normal API:

Frontend
 ↓
Backend
 ↓
Validate request
 ↓
Database

You wouldn't normally trust arbitrary frontend input.

Likewise:

LLM
 ↓
Validate output
 ↓
Application

Your application should verify:

required fields exist
correct types
valid enum values
valid ranges
business rules

For example:

amount > 0

might be a business rule.

The LLM shouldn't be the final authority on that.

9. Structured output does NOT mean correct output

This is extremely important.

Suppose the model returns:

{
  "customerName": "Ahmed",
  "amount": 500
}

The JSON might be perfectly valid.

But perhaps Ahmed actually owes:

$270

So:

Valid structure ≠ correct information.

Structured output solves primarily a format/reliability problem, not a truth problem.

Remember our previous lesson about hallucinations.

10. Structured output + tools

This becomes even more powerful when combined with tool calling.

Imagine your user says:

"Check Ahmed's balance."

The LLM could produce a structured tool request:

{
  "customerId": "123",
  "action": "get_balance"
}

Your application validates it:

LLM
 ↓
Structured tool request
 ↓
Validation
 ↓
Permission check
 ↓
Database
 ↓
$270
 ↓
LLM
 ↓
User

This is one of the foundations of AI agents.

11. Structured output in a real SaaS

Imagine Mizan has customer debts.

A customer sends:

"Ahmed owes 50 dollars and will pay tomorrow."

Your AI system could extract:

{
  "customerName": "Ahmed",
  "amount": 50,
  "currency": "USD",
  "dueDate": "tomorrow"
}

Your application can then:

Validate
   ↓
Resolve "tomorrow"
   ↓
Find customer
   ↓
Check permissions
   ↓
Save/update debt

Notice something important:

The LLM isn't directly controlling your database.

Your software remains in control.

The LLM produces structured information, and your application decides what to do with it.

12. This is where AI engineering meets normal engineering

Your existing backend knowledge becomes very useful here.

You already understand concepts like:

APIs
DTOs
validation
schemas
databases
authentication
authorization
business rules

Now you're adding:

LLM
 ↓
Structured output
 ↓
Validation
 ↓
Normal application logic

So you don't need to throw away your software-engineering knowledge to become an AI Engineer.

You're extending it.

13. Structured output vs function/tool calling

These concepts are related but different.

Structured output

The model returns structured data.

{
  "customer": "Ahmed",
  "amount": 270
}

Your application decides what to do with it.

Tool calling

The model requests that your application execute a specific tool.

Conceptually:

{
  "tool": "getCustomerBalance",
  "arguments": {
    "customerId": "123"
  }
}

Then your application executes:

getCustomerBalance("123")

We'll study tool calling later in the Agents section.

14. The mental model

Keep this:

                  USER
                    ↓
                  LLM
                    ↓
           Structured output
                    ↓
                Validate
                    ↓
           Application logic
                    ↓
              Database/API

The LLM provides language intelligence.

Your application provides:

rules
permissions
validation
persistence
deterministic operations

This separation is extremely important for production AI systems.

15. One final distinction

There are three different things you should now keep separate:

Free-form generation
"Ahmed owes $270."

Good for humans.

Structured output
{
  "customer": "Ahmed",
  "amount": 270
}

Good for software.

Tool calling
{
  "tool": "getCustomerBalance",
  "arguments": {
    "customerId": "123"
  }
}

Good when the model needs your application to perform an operation.

These will eventually fit together in your AI applications.

🧠 Layer 1 progress

You've now covered:

✅ Tokens & tokenization
✅ Context windows
✅ Transformers
✅ Attention
✅ Parameters & inference
✅ Temperature & top-p
✅ Model limitations & hallucinations
✅ Structured outputs / JSON

Only one concept remains: