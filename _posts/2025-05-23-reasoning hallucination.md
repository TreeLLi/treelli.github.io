---
layout: distill
title: Are Reasoning Models Hallucinating More?
description: OpenAI appears to have tuned its latest models—o3-pro, o4-mini, and GPT-4o—to be overconfident, forcing them to answer questions they don’t actually understand.
giscus_comments: true
date: 2025-06-20
tags: Report
categories:

authors:
  - name: Lin Li
    url: "https://treelli.github.io"
    affiliations:
      name: OATML, Oxford

bibliography: blog.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: More Talk, More Mistakes
  - name: Is Reasoning the Culprit?
  - name: Final Thoughts

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---

According to OpenAI’s own system cards <d-cite key="openai2025o3"></d-cite> and evaluations <d-cite key="openai2025eval"></d-cite>, **their most recent reasoning models—o3 and o4-mini—hallucinate more frequently than their predecessors, o1 and o3-mini** (see Figures below). Independent benchmarks, including Google’s FACTS Grounding <d-cite key="jacovi2025facts"></d-cite> and Advaameg’s LLM Confabulation Leaderboard <d-cite key="lechmazurconfabulations"></d-cite>, have reported similar findings, pointing to a broader, more persistent issue across the data and tasks. This trend has sparked widespread public concern, as covered by major outlets like [_The New York Times_](https://www.nytimes.com/2025/05/05/technology/ai-hallucinations-chatgpt-google.html), [_TechCrunch_](https://techcrunch.com/2025/04/18/openais-new-reasoning-ai-models-hallucinate-more/), and [_Forbes_](https://www.forbes.com/sites/conormurray/2025/05/06/why-ai-hallucinations-are-worse-than-ever/). Some have even interpreted it as a sign that progress in AI may have already hit a wall.

Just a few days ago, OpenAI launched its most powerful model yet: o3-pro. But despite the buzz, third-party evaluations <d-cite key="lechmazurconfabulations"></d-cite><d-cite key="superficial2025"></d-cite> show that o3-pro performs more or less on par with o3 when it comes to hallucination and non-response rates. In other words, **o3-pro still hallucinates much more than o1**. That makes it clear this isn’t an easy issue to fix—and it might stick around longer than anyone hoped.

<div class="page">
  <iframe src="{{ '/assets/plotly/sim_per_qa.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="justify-content: center"></iframe>
</div>

## More Talk, More Mistakes

The underlying cause appears straightforward: **the newer models are more confident, even when they shouldn’t be**. Compared to earlier models, they are inclined to answer a question rather than admit uncertainty—something reflected in their lower non-response rates (the gray bars). As a result, we see more answers overall, both right and wrong. That’s why **both accuracy and hallucination rates go up at the same time**. The problem is, these forced answers are more likely to be wrong than right, which means **the rise in hallucinations ends up outpacing any gains in accuracy**. The trends are even more pronounced on the PersonQA dataset. (You can switch the figure above to this dataset by selecting it from the dropdown menu at the top right corner.)

A similar trend of rising overconfidence and hallucination <d-footnote>Hallucination here is defined as an LLM providing an answer to a question that does not have a valid answer based on the given context. This differs from the definition used in OpenAI's own benchmarks. Note that the definition of non-response is also slightly different in this evaluation. For more details, please refer to the original source: https://github.com/lechmazur/confabulations.</d-footnote> shows up in third-party evaluations on the LLM Confabulation Leaderboards. Interestingly, almost all OpenAI models show significantly lower non-response rates and simultaneously higher hallucination rates compared to the latest Google Gemini and Anthropic Claude models, suggesting this seems to be **a localised issue with OpenAI’s models**. **The latest Gemini and Claude models are generally more cautious**: they refuse to answer ambiguous or uncertain queries more often, therefore they hallucinate less.

<div class="page">
  <iframe src="{{ '/assets/plotly/llmconfab.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="justify-content: center"></iframe>
</div>

## Is Reasoning the Culprit?

Probably not. The tendency to hallucinate isn’t limited to OpenAI’s reasoning-focused models. Even GPT-4o, which is not designed for complex reasoning, is showing similar behaviour: fewer refusals to answer, but more hallucinations. This points to **a deeper issue that likely doesn’t come from the reasoning mechanism itself, but rather from something earlier in the training pipeline**. We can further narrow down the scope to something not done in GPT-4.5 because it manages to reduce the non-response rate while also lowering hallucinations, indicating more genuine confidence. So what exactly did OpenAI change in their latest training or alignment process? And why? These are key questions that remain unanswered—though it’s likely some researchers at OpenAI are already trying to figure that out.

## Final Thoughts

OpenAI’s recent overconfidence issue is a reminder of **how delicate the trade-offs in LLM development can be**. Making a model “more helpful” can easily tip into making it “more wrong.” Progress doesn’t always come in a straight line—and sometimes, unintended consequences sneak in through the very improvements we aim to make.