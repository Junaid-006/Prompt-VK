# Task 2 -- Prompt Engineering Mastery

## 1. Overview

**Objective:** Demonstrate three prompt-engineering techniques through
hands-on experiments: zero-shot vs. few-shot prompting, step-by-step
reasoning prompts, and structured JSON output.

**Platform:** Google AI Studio\
**Model selected:** Gemini 4 Hydrogen (model name recorded as
selected/reported during the experiments)\
**Programming required:** None for the experiments documented here.

The report records the prompts and model responses captured during the
experiments. The outputs are reproduced as supplied, with formatting
normalised only where necessary for Markdown readability.

------------------------------------------------------------------------

## 2. Experiment 1 -- Zero-shot vs. Few-shot Prompting

### 2.1 Task and evaluation

The task was to classify five customer reviews as **Positive**,
**Negative**, or **Neutral**.

The same five test reviews were used for both runs. The expected labels
were:

  -----------------------------------------------------------------------
  Review                              Expected label
  ----------------------------------- -----------------------------------
  1\. "The delivery was incredibly    Positive
  fast and the product works          
  perfectly!"                         

  2\. "The quality is terrible. I     Negative
  regret buying this."                

  3\. "The package arrived yesterday. Neutral
  Nothing special."                   

  4\. "Amazing experience! I will     Positive
  definitely recommend this to        
  everyone."                          

  5\. "The product looks good, but    Negative
  the battery drains very quickly."   
  -----------------------------------------------------------------------

### 2.2 Test A -- Zero-shot prompt

**Prompt used:**

``` text
You are a sentiment classification system.

Classify each customer review into exactly one category:
- Positive
- Negative
- Neutral

Return the result as a numbered list containing the review number and its sentiment. Do not provide explanations.

Reviews:

1. "The delivery was incredibly fast and the product works perfectly!"
2. "The quality is terrible. I regret buying this."
3. "The package arrived yesterday. Nothing special."
4. "Amazing experience! I will definitely recommend this to everyone."
5. "The product looks good, but the battery drains very quickly."
```

**Actual model output:**

``` text
1.
   Positive
2. Negative
3. Neutral
4. Positive
5. Negative
```

**Result:** 5 out of 5 classifications were correct.

### 2.3 Test B -- Few-shot prompt

**Prompt used:**

``` text
You are a sentiment classification system.

Classify customer reviews into exactly one category:
- Positive
- Negative
- Neutral

Here are some examples:

Example 1:
Review: "I absolutely love this product!"
Sentiment: Positive

Example 2:
Review: "The product stopped working after two days."
Sentiment: Negative

Example 3:
Review: "The item arrived today as expected."
Sentiment: Neutral

Example 4:
Review: "Excellent quality and fantastic customer service!"
Sentiment: Positive

Example 5:
Review: "Very disappointing. Not worth the money."
Sentiment: Negative

Now classify these reviews:

1. "The delivery was incredibly fast and the product works perfectly!"
2. "The quality is terrible. I regret buying this."
3. "The package arrived yesterday. Nothing special."
4. "Amazing experience! I will definitely recommend this to everyone."
5. "The product looks good, but the battery drains very quickly."

Return only a numbered list with the sentiment.
```

**Actual model output:**

``` text
1. Positive
2. Negative
3. Neutral
4. Positive
5. Negative
```

### 2.4 Comparison and analysis

  Measure                       Zero-shot   Few-shot
  --------------------------- ----------- ----------
  Correct classifications             5/5        5/5
  Accuracy on this test set          100%       100%
  Label format followed               Yes        Yes

Both approaches classified all five reviews correctly. Few-shot
prompting did not increase measured accuracy because the zero-shot
prompt already achieved 5/5. The labelled examples nevertheless made the
intended categories and classification pattern explicit. A larger test
set containing more ambiguous or mixed-sentiment reviews would be needed
to determine whether few-shot prompting improves accuracy or consistency
in harder cases.

**Finding:** In this small experiment, few-shot prompting did not
improve accuracy over zero-shot prompting.

------------------------------------------------------------------------

## 3. Experiment 2 -- Step-by-step Prompting

### 3.1 Task and evaluation

The reasoning task was a multi-step profit-and-loss word problem.

**Problem used in both runs:**

> A shopkeeper buys 40 notebooks for ₹25 each. He sells 25 notebooks for
> ₹35 each and the remaining notebooks for ₹20 each. What is his total
> profit or loss?

**Verified answer:**

-   Total cost price = 40 × ₹25 = ₹1,000
-   Selling price of first 25 notebooks = 25 × ₹35 = ₹875
-   Remaining notebooks = 40 − 25 = 15
-   Selling price of remaining 15 notebooks = 15 × ₹20 = ₹300
-   Total selling price = ₹875 + ₹300 = ₹1,175
-   Profit = ₹1,175 − ₹1,000 = **₹175**

### 3.2 Test A -- Direct prompt

**Prompt used:**

``` text
A shopkeeper buys 40 notebooks for ₹25 each.
He sells 25 notebooks for ₹35 each and the remaining notebooks for ₹20 each.

What is his total profit or loss?
```

**Actual model output:**

``` text
To find the total profit or loss, let's calculate the total cost price and the total selling price.

1. Total Cost Price (CP):

- Number of notebooks = 40
- Cost per notebook = ₹25

Total CP = 40 × 25 = ₹1,000

2. Total Selling Price (SP):

- First 25 notebooks sold at ₹35 each:
  25 × 35 = ₹875

- Remaining notebooks:
  40 − 25 = 15 notebooks

- Remaining 15 notebooks sold at ₹20 each:
  15 × 20 = ₹300

Total SP = 875 + 300 = ₹1,175

3. Profit or Loss:

Since the Total Selling Price (₹1,175) is greater than the Total Cost Price (₹1,000), the shopkeeper makes a profit.

Profit = Total SP − Total CP
Profit = 1,175 − 1,000 = ₹175

The shopkeeper's total profit is ₹175.
```

### 3.3 Test B -- Step-by-step prompt

**Prompt used:**

``` text
Solve the following mathematical word problem.

Before giving your final answer:
1. Identify the given information.
2. Calculate the total cost price.
3. Calculate the total selling price for both groups.
4. Compare the total cost price and selling price.
5. Determine whether there is a profit or loss and calculate the amount.

Present a concise explanation of your calculations and clearly state the final answer.

Problem:

A shopkeeper buys 40 notebooks for ₹25 each.
He sells 25 notebooks for ₹35 each and the remaining notebooks for ₹20 each.

What is his total profit or loss?
```

**Actual model output:**

``` text
1. Given Information:

  - Total notebooks bought: 40
  - Cost price per notebook: ₹25
  - First group sold: 25 notebooks at ₹35 each
  - Remaining notebooks sold: 40 - 25 = 15 notebooks at ₹20 each

2. Total Cost Price (CP): \text{Total CP} = 40 \times ₹25 = ₹1,000

3. Total Selling Price (SP):

  - Selling price of the first group: 25 \times ₹35 = ₹875
  - Selling price of the remaining group: 15 \times ₹20 = ₹300
    \text{Total SP} = ₹875 + ₹300 = ₹1,175

4. Comparison: \text{Total SP (₹1,175)} > \text{Total CP (₹1,000)}

5. Profit or Loss: Since the total selling price is greater than the total cost
price, the shopkeeper makes a profit. \text{Profit} = ₹1,175 - ₹1,000 = ₹175

Final Answer: The shopkeeper makes a total profit of ₹175.
```

### 3.4 Comparison and analysis

  Measure                    Direct prompt           Step-by-step prompt
  -------------------------- ----------------------- --------------------------------
  Final answer               ₹175 profit             ₹175 profit
  Mathematical correctness   Correct                 Correct
  Explanation                Included calculations   Followed five requested stages
  Requested structure        Not specified           Explicitly specified

The direct prompt unexpectedly produced a detailed calculation even
though it did not explicitly request one. The step-by-step prompt
followed the requested five-stage structure and made the solution easier
to inspect. However, it did not improve accuracy in this example because
both responses reached the correct answer. This experiment therefore
shows an improvement in response organisation, not a demonstrated
improvement in mathematical accuracy.

**Finding:** Explicit step-by-step instructions made the response
structure more predictable, but did not change the correct final answer
in this test.

------------------------------------------------------------------------

## 4. Experiment 3 -- Structured JSON Output

### 4.1 Task and schema

The task was to extract three fields from invoice-like text:

-   `name`: Customer's name
-   `date`: Invoice date in `YYYY-MM-DD` format
-   `amount`: Total amount as a JSON number

**Required JSON structure:**

``` json
{
  "name": "string",
  "date": "YYYY-MM-DD",
  "amount": 0.0
}
```

### 4.2 Initial prompt and instructions

**Prompt used for the first input:**

``` text
You are an invoice data extraction system.

Extract the following information from the given text:
- name: Customer's name
- date: Invoice date in YYYY-MM-DD format
- amount: Total invoice amount as a number

Return ONLY valid JSON using exactly this structure:

{
  "name": "string",
  "date": "YYYY-MM-DD",
  "amount": 0.0
}

Rules:
1. Do not include explanations.
2. Do not include Markdown code fences.
3. Use double quotes for JSON keys and string values.
4. Amount must be a number, not a string.
5. Do not invent missing information.
6. If a field is missing, use null.

Invoice text:

"Invoice issued to Rahul Sharma on 15 November 2026. The total amount payable for the purchased items is Rs. 4,850 only."
```

For Tests 2--5, the same chat and previously supplied schema and rules
were retained. Each subsequent input asked the model to extract the
information and return only JSON using the exact schema and rules
already provided.

### 4.3 Test inputs and actual outputs

#### Test 1

**Input:**

``` text
Invoice issued to Rahul Sharma on 15 November 2026. The total amount payable for the purchased items is Rs. 4,850 only.
```

**Actual output:**

``` json
{ "name": "Rahul Sharma", "date": "2026-11-15", "amount": 4850 }
```

**Validation:** All three fields correct; valid JSON.

#### Test 2

**Input:**

``` text
Invoice details:
Customer: Priya Nair
Date of issue: 22 October 2026
Items purchased: Office supplies
Grand total payable: INR 12,750.50
```

**Actual output:**

``` json
{ "name": "Priya Nair", "date": "2026-10-22", "amount": 12750.50 }
```

**Validation:** All three fields correct; valid JSON.

#### Test 3

**Input:**

``` text
Invoice #INV-3098

Billed to: Mohammed Irfan
This invoice was generated on December 3, 2026.
The customer purchased electronic accessories.
Amount due after all charges: Rs. 7,299.
```

**Actual output:**

``` json
{ "name": "Mohammed Irfan", "date": "2026-12-03", "amount": 7299 }
```

**Validation:** All three fields correct; valid JSON.

#### Test 4

**Input:**

``` text
Hey, just sharing the bill details.

Customer name - Ananya Krishnan
Bill generated: 7th September 2026
Total payable (including tax): ₹18,499.75
```

**Actual output:**

``` json
{ "name": "Ananya Krishnan", "date": "2026-09-07", "amount": 18499.75 }
```

**Validation:** All three fields correct; valid JSON.

#### Test 5

**Input:**

``` text
TAX INVOICE

Recipient: Arjun Mehta

Issued on: 18th November 2026

Description: Laptop accessories and peripherals

Final amount payable: INR 1,25,999.00

Payment status: Pending
```

**Actual output:**

``` json
{ "name": "Arjun Mehta", "date": "2026-11-18", "amount": 125999.00 }
```

**Validation:** All three fields correct; valid JSON.

### 4.4 Results

  -----------------------------------------------------------------------
  Test        Customer    Correct     Correct     Valid JSON  Result
                          date        amount                  
  ----------- ----------- ----------- ----------- ----------- -----------
  1           Rahul       Yes         Yes         Yes         Pass
              Sharma                                          

  2           Priya Nair  Yes         Yes         Yes         Pass

  3           Mohammed    Yes         Yes         Yes         Pass
              Irfan                                           

  4           Ananya      Yes         Yes         Yes         Pass
              Krishnan                                        

  5           Arjun Mehta Yes         Yes         Yes         Pass
  -----------------------------------------------------------------------

**Success rate:** 5/5 = **100%**

All 15 extracted field values were correct across the five tests, and
all five responses followed the requested JSON structure.

### 4.5 Analysis

The explicit schema, field descriptions, date-format requirement,
numeric amount requirement, and instruction to return JSON only helped
produce consistent outputs. The model handled several variations in
wording, date presentation, currency notation, and amount formatting.
However, this was a small test set, and the results do not establish
reliability across all invoice formats. The experiment used prompt-based
JSON generation; a dedicated JSON-mode or schema-constrained API feature
was not tested.

**Finding:** Explicit schema instructions and formatting rules produced
valid, correct JSON in all five test cases.

------------------------------------------------------------------------

## 5. Overall Results

  Experiment                             Tests Result
  ------------------------------------ ------- ----------------
  Zero-shot sentiment classification         5 5/5 correct
  Few-shot sentiment classification          5 5/5 correct
  Direct mathematical prompt                 1 Correct
  Step-by-step mathematical prompt           1 Correct
  Structured JSON extraction                 5 5/5 successful

### Key observations

1.  **Zero-shot vs. few-shot:** Both approaches achieved 100% accuracy
    on the five selected reviews. Few-shot examples did not improve the
    measured accuracy in this test.
2.  **Step-by-step prompting:** Both prompts produced the correct profit
    of ₹175. The explicit instructions improved the organisation of the
    response, but not its accuracy.
3.  **Structured output:** The model returned valid JSON with all
    expected field values in all five invoice tests. Explicit schema and
    formatting instructions helped maintain consistency.

## 6. Conclusion

These experiments demonstrated three practical prompt-engineering
techniques using Google AI Studio.

Zero-shot prompting was sufficient for the selected
sentiment-classification examples, while few-shot prompting provided
additional examples of the expected behaviour without increasing
accuracy on this small dataset. Step-by-step instructions made the
mathematical explanation more structured, although both direct and
structured prompts produced the same correct result. Finally, explicit
JSON schema instructions enabled consistent extraction of names, dates,
and amounts across five differently worded invoice descriptions.

The experiments also highlighted the importance of evaluating prompts
using actual outputs rather than assuming that a more elaborate prompt
will always perform better. The results are encouraging, but the small
datasets mean that further testing with more varied and ambiguous inputs
would be needed before making broader reliability claims.

------------------------------------------------------------------------

## 7. Reproducibility Notes

-   Platform: Google AI Studio
-   Model selected: Gemini 4 Hydrogen (as reported during the
    experiment)
-   The same selected model was used for the experiments.
-   The prompts and outputs in this report were captured during the
    hands-on tests.
-   No dedicated JSON-mode or schema-constrained API feature was used in
    Experiment 3.
-   Accuracy figures refer only to the small test sets documented in
    this report.
