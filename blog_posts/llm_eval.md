&leftarrow; [back to Homepage](../index.md)

# Could a language model graduate high school in Romania in 2024?

TLDR: No, or at least no in one-shot.

* [Intro](#intro)
* [The models](#the-models)
* [The test](#the-test)
* [The prompt](#the-prompt)
* [The results](#the-resutls)
* [Philosophical digression](#philosophical-digression)
* [Full chat logs](#full-chat-logs)

## Intro
We are living in exciting times, as ever more capable language models are being released. However, the biggest pain point for me with language models is that it's really hard to objectively evaluate these models. Many of the standard benchmarks used are of questionable usefulness and some are also suspect of being leaked in the training data. So we are left wondering how good these models really are?

Well, I decided to do my own little evaluation experiment. As the high school graduation exams just finished in Romania, I was curious to put the two most hyped language models to the test, and see how they would perform.

## The models

The models I've chosen to evaluate are:
- [ChatGPT 4o](https://openai.com/index/gpt-4o-and-more-tools-to-chatgpt-free) - (by OpenAI released on May 13, 2024)
- [Claude 3.5 Sonnet](https://www.anthropic.com/news/claude-3-5-sonnet) - (by Anthropic released on June 21, 2024)

Why these models?
- they have been recently release, and in close proximity to each other
- they were developed by two of the leading "AI" companies
- they make bold promises

## The test

The test I've used is a standardized exam called "Bacalaureat" (BAC for short), taken at high school graduation in several subjects, nationwide in Romania. Passing the exam is generally required for admission to higher education. From all possible subjects, I've settled on using the math exam, as it is objective to evaluate and the math syntax and grammar are international, easy to understand for anyone.

There are several levels at which the math exam can be taken (M1,M2,M3,M4), and I went for the second hardest (M2), which was given to students who studied in classes focused on natural sciences. I did not go for the most advanced level (M1), as I don't think I could have fully understood the solutions and verified it myself.

These exams are published in several different languages, for all the significant minorities living in Romania, however they are not published in English. So just to keep things simple for myself, and to give an extra challenge to the language models, I've left the exam questions in the original Romanian language.

As for the evaluation, I've used the official solution and scoring guides published by the government, and I went through the language model answers, manually, to grade the solutions myself.

Why the Math M2 BAC 2024 exam in Romanian?
- because it's a standardized test, with official scoring guidelines
- that was published and written recently (so low probability of leakage in the training data) by high school graduate students
- because it's objective, and international
- the M2 level is easier for me to understand and I'm able to follow the solutions and evaluate them manually
- a multilingual math tests is more challenging, however math equations are language independent, so this should not be a huge obstacle for these models

Links:
- exam questions - Math M2 BAC 2024
- evaluation and grading guide - Math M2 BAC 2024

## The prompt

I've uploaded the pdf file containing the exam questions and user just one, simple zero-shot prompt to ask the language model to solve the exam:

```
Hello, I want to test your knowledge on a standardized mathematics test. I'm going to share with you the exam questions and expect you to solve them to the best of your knowledge. The questions are in Romanian, and I will expect the answers also to be in Romanian. This is an extra complication, but the reason for it is that I want to test your foreign language understanding as well. And finally, it's okay to skip a question or not to answer a questions if you don't know how. Best of luck!
```

## The results

Drumroll!

- ChatGPT-4o - 25%
- Claude 3.5 Sonnet - 43%

Okay, I have to admit that I expected more, I expected these language models to pass the exam (by scoring more than 50%). But probably this is me having unrealistic expectations, because of all the hype around language models.

Thinking about this rationally, this is an exceptionally hard task. Multilingual, high school math exam, on the first try, using the one prompt and the raw exam file - with all the intricate math notation (square roots, integrals, matrices). Given all this, this is a great try on the first attempt. And I'm being honest, I would have scored lower on a first attempt, without preparation.

Iterating on the prompts, asking follow-up questions and guiding the models, probably would result in an excellent score. However, I don't have time for that experiment this time.

Here are the breakdowns for the points scored on each exercise by the models. The main takeaway is that many mistakes can be attributed to the models inability to correctly read math syntax from the pdf file, resulting in incorrect formulas and calculations. Probably this is something that will greatly improve in the future.

## Philosophical digression

So a language model cannot graduate high school in Romania. Or at least not yet. Phew, that's reassuring. But let's imagine for a moment that it could. Let's assume that next year or the year after, the same evaluation will result in test scores of 99%.

What would that mean? Would language models be smarter than high school students?
Not necessarily. Is a calculator smarter than me, because it can calculate the nth root of a number, which I could not? Standardized tests offer a common way to evaluate and rank students, but just as any kind of evaluation, they are not perfect. They are a tool to measure some skills, because the education system needs to somehow rank students, but they do not measure all skills.

But is there any value in making students write an exam, which could be aced by a language model? There might be some value to it still. Is there any value in learning and playing chess, even though the very best players (by far) are computer algorithms? Chess is still a popular game with a huge fanbase, and playing chess improves skills which can be used in other areas of life (focus ability, pattern matching, memorisation). The same way, exams where language models are superior, could still be beneficial to sharpen skills for us, little humans.

## Full chat logs
