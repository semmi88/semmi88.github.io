&leftarrow; [back to Homepage](../index.md)

# Could a language model graduate high school in Romania in 2024?

**TLDR**: No. Or at least not in one-shot.

* [Intro](#intro)
* [The models](#the-models)
* [The test](#the-test)
* [The prompt](#the-prompt)
* [The results](#the-results)
* [Philosophical digression](#philosophical-digression)
* [Full chat logs for ChatGPT](#full-chat-logs-for-chatgpt)
* [Full chat logs for Claude](#full-chat-logs-for-claude)
* [Screenshot of ChatGPT taking the exam - partial](#screenshot-of-chatgpt-taking-the-exam---partial)
* [Screenshot of Claude taking the exam - partial](#screenshot-of-claude-taking-the-exam---partial)

## Intro
We are living in exciting times, as ever more capable language models are being released. However, the biggest pain point for me with language models is that it's really hard to objectively evaluate these models. Many of the standard benchmarks used are of questionable usefulness and some are also suspect of being leaked in the training data. So we are left wondering how good these models really are?

Well, I decided to do my own little evaluation experiment. As the high school graduation exams just finished in Romania, I was curious to put the two most hyped language models to the test, and see how they would perform.

![alt text](robo_exam.png "A robot taking an exam")

(image generated by [hotpot.ai](hotpot.ai/art-generator))

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
- [official goverment website for the Bacalaureat exam](http://subiecte2024.edu.ro/2024/bacalaureat/Subiecte_si_bareme/)
- exam questions reuploaded to github - [Math M2 BAC 2024](https://github.com/semmi88/semmi88.github.io/blob/master/blog_posts/mat_st-nat_2024_LRO.pdf)
- evaluation and grading guide reuploaded to github - [Math M2 BAC 2024 eval](https://github.com/semmi88/semmi88.github.io/blob/master/blog_posts/mat_st-nat_2024_bar_LRO.pdf)

## The prompt

I've uploaded the pdf file containing the exam questions and used just one, simple zero-shot prompt to ask the language model to solve the exam:

```
Hello, I want to test your knowledge on a standardized mathematics test.
I'm going to share with you the exam questions and expect you to solve them to the best of your knowledge.
The questions are in Romanian, and I will expect the answers also to be in Romanian.
This is an extra complication, but the reason for it is that I want to test your foreign language understanding as well.
And finally, it's okay to skip a question or not to answer a questions if you don't know how.
Best of luck!
```

## The results

**Drum roll!**

The exam scores go from 10 to 100 points.
- **ChatGPT-4o** scored **25 points**
- **Claude 3.5 Sonnet** scored **43 points**


Okay, I have to admit that I expected more, I expected these language models to pass the exam (by scoring more than 50 points, although Claude got close to this). But probably this is me having unrealistic expectations, because of all the hype around language models.

Thinking about this rationally, this is an exceptionally hard task. Multilingual, high school math exam, on the first try, using just one prompt and the raw exam file - with all the intricate math notation (square roots, integrals, matrices). Given all this, this is a great try on the first attempt. The questions were understood by the models (despite being in Romanian and using lots of math notation), and the answers were coherent and some of the answers followed the correct math calculation and reasoning. 

These low exam scores could probably be drastically improved, by iterating on the prompts, asking follow-up questions and guiding the models. However, I don't have time for that experiment this time.

[Here](https://github.com/semmi88/semmi88.github.io/blob/master/blog_posts/llm_eval_scores.pdf) are the breakdowns for the points scored on each exercise by the models. The main takeaway is that many mistakes can be attributed to the inability of both models to correctly read math syntax from the pdf file, resulting in incorrect formulas and calculations. Probably this is something that will greatly improve in the future.

Some interesting differences:
- ChatGPT understood the task right away, while Claude needed an extra prompt to start solving the exam
- ChatGPT gave answers in a friendlier, "mathy" format
- ChatGPT skipped many of the questions, which contributed to its worse performance. (On the other hand this was instructed in the propmt and inadvertedly could have impacted the final results)
- Claude answered the questions by sections, and asked for feedback to continue on to the next section
- Claued concluded in two cases that solving the equation is too complex in the limited time of an exam :) 

![alt text](llm_eval_scores.png "Exam scores for the two language models")


## Philosophical digression

So a language model cannot graduate high school in Romania. Or at least not yet. Phew, that's reassuring. But let's imagine for a moment that it could. Let's assume that next year or the year after, the same evaluation will result in test scores of 99%.

What would that mean? Would language models be smarter than high school students?
Not necessarily. Is a calculator smarter than me, because it can calculate the nth root of a number, which I could not? Standardized tests offer a common way to evaluate and rank students, but just as any kind of evaluation, they are not perfect. They are a tool to measure some skills, because the education system needs to somehow rank and grade students, but they do not measure all skills.

But is there any value in making students write an exam, which could be aced by a language model? There might be some value to it still. Is there any value in learning and playing chess, even though the very best players (by far) are computer algorithms? Chess is still a popular game with a huge fanbase, and playing chess improves skills which can be used in other areas of life (focus ability, pattern matching, memorisation). The same way, exams where language models are superior, could still be beneficial to sharpen skills for us, little humans.

## Full chat logs for ChatGPT

- [OpenAI shareble link - ChatGPT multilingual math exam chat](https://chatgpt.com/share/ad0aa15a-866b-4089-8a6e-17dd321ef752)
- [Markdown verison](https://github.com/semmi88/semmi88.github.io/blob/master/blog_posts/chatgpt_full_math_exam_chat_log.md)

## Full chat logs for Claude

- No shareable link from Anthropic :(
- [AIArchives link to Claude multilingual math exam chat](https://aiarchives.org/id/yEYka1cLaJqBqobx5BOV)
- [Markdown version](https://github.com/semmi88/semmi88.github.io/blob/master/blog_posts/claude_full_math_exam_chat_log.md)

## Screenshot of ChatGPT taking the exam - partial

![alt text](chatgpt_math_exam.png "ChatGPT math exam prompt and answer")

## Screenshot of Claude taking the exam - partial

![alt text](claude_math_exam.png "Claude math exam prompt and answer")
