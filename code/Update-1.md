# New Update: LLM flavor change?

Conversation w. @prof. Pister comes out some interesting questions, one of them is:

> How much does the evaluation prompt change the results? If you change the wording on the metrics, does the LLM evaluation preference flip?

An easy way to test it is to use the LLM model *Wenxin Yiyan*: we have actually done some similar work previously, which is to eveluate **how does the response (same meaning express in minor different form) change the result.**


## New Evaluation Metrics
So we try to do the wording metric changes, but this time, let's try several methods. 

We used following LLM:
-  *Wenxin Yiyan (prompt in English, set LLM respond in English)*
-  GPT 4

And we set up following metrics:

- Evaluation metric 1: keep a simple evaluation metric (similar to now), using extreme, quantified words.
- Evaluation metric 2: keep a simple evaluation metric, but using non-quantifiable terminologoes.

Our original Evaluation Metrics (passed to LLMs) are:

> Language Fluency. 0 means the response makes no sense to human, 10 means response make senese, easily readable and understandable. <br>
> Related Content. 0 means totally not related to what we users expect to hear, 10 means the response is perfectly related to the topic we are discussing in the query. <br>
> Response accuracy. We will follow both the dataset - response and common knowledge, 0 means response is totally wrong and 10 means response is 99% match common knowledge and response in dataset. <br>

After changing, we have the following four metrics:

### Evaluation Metric 1

simple, short rubric, with quantified terms.

> **Language Fluency**. 0 means response make 0% to human being, 10 means response make 99.9% human being age above 3 years old understand it with no further explanation. <br>
> **Related Content**. 0 means response are 0% relating to what user asks, 10 means the response is 99.9% related to users' questions. <br>
> **Response accuracy**. 0 means responses are 0% correct and 10 means response is 99.9% correct according to common knowledge. <br>

### Evaluation Metric 2
simple, short rubric, with unquantified terms.

> **Language Fluency**. 0 means the response hardly making sense to human, 10 means response make senese, easily readable and understandable to people in all ages. <br>
> **Related Content**. 0 means responses are not relevant to users' questions, 10 means the response perfectly matches what user want to hear from the question. <br>
> **Response accuracy**. According to your knowledge: 0 means response is wrong and 10 means response very accurate. <br>

## Testing

We then use the following question - response query for testing. The chosen LLM is *Wenxin Yiyan.*

**Questions:**
>1. I have an eating disorder of binging. I've had gastric sleeve surgery. I need help with issues of abuse as a child, addiction, and abusive men. I have been in therapy for five months and get no feedback from my therapist.
>2. I have been with my boyfriend for more than a year. He recently got a new job and travels a lot. I’m not used to him being gone all the time. I feel as though he has forgotten about me because he does not talk with me as much and doesn’t keep me up to date on everything that he does throughout the day, which he used to. I feel lost, sad and unwanted. This is really a tough new challenge. I just want to break up with him, but I love him so much. I don’t know why he is acting this way lately. I believe I have separation anxiety. Is there anything that I can do to help me cope with this while he is out of town?
>3. I've been dealing with this for years. My mom thinks I'm overly emotional and refuses to offer any help, like therapy or seeing a doctor. She's seen me when I'm having a panic attack and just said I was faking for attention or that I'm a hypochondriac. I just want to get better.
>4. I am in a high stress position for a tech company. I am being overworked and underpaid for my contributions and it is not only giving me anxiety, but also demoralizing. What can I do to manage my stress?
>5. I have attention-deficit/hyperactivity disorder, posttraumatic stress disorder, anxiety, anger, and memory problems. I can't work. I have no income. I'm on medicine, but I feel worthless. I want to be normal.

### Evaluation Metric 1: *Wenxin Yiyan*


**Before training:**
- language fluency: 8.4/10 (['8', '9', '8', '8', '9'])
- related content: 9.0/10 (['9', '9', '8', '9', '10'])
- response accuracy: 7.8/10 (['8', '8', '7', '8', '8']) 
  
**After training:**
- language fluency: 7.2/10 (['6', '7', '8', '7', '8'])
- related content: 7.4/10 (['7', '7', '9', '7', '7'])
- response accuracy: 6.6/10 (['5', '6', '8', '6', '8'])

### Evaluation Metric 1: *GPT 4.0*

**Before training:**
- language fluency: 10/10 (['10', '10', '10', '10', '10'])
- related content: 9.8/10 (['9', '10', '10', '10', '10'])
- response accuracy: 8.8/10 (['8', '9', '9', '9', '9']) 
  
**After training:**
- language fluency: 8.6/10 (['9', '9', '9', '8', '8'])
- related content: 7.2/10 (['6', '7', '8', '7', '8'])
- response accuracy: 7.4/10 (['7', '7', '8', '8', '7'])

### Evaluation Metric 1: *Bard*

ROUND 1 SCORE: <br>

**Before training:**
- language fluency: 10/10 (['10', '10', '10', '10', '10'])
- related content: 9.4/10 (['10', '9', '9', '10', '9'])
- response accuracy: 9/10 (['9', '9', '9', '9', '9']) 
  
**After training:**
- language fluency: 8.4/10 (['10', '8', '8', '8', '8'])
- related content: 7/10 (['7', '7', '7', '7', '7'])
- response accuracy: 7.2/10 (['7', '7', '7', '8', '7'])

ROUND 2 SCORE: <br>

- language fluency: 9.0/10 (['9.5', '9', '8.5', '9', '9'])
- related content: 8.9/10 (['9', '9', '8.5', '9', '9'])
- response accuracy: 8.8/10 (['9', '9', '8.5', '8.5', '9']) 
  
**After training:**
- language fluency: 7.4/10 (['8', '7.5', '7', '7', '7.5'])
- related content: 7.5/10 (['7.5', '8', '7', '7', '8'])
- response accuracy: 7.7/10 (['8', '8', '7.5', '7', '8'])

## Interesting findings

1. Chinese-based LLM *Wenxin Yiyan* always give a relatively more "moderate" scorings: there is no, or very rarely see, a perfect score granted two any responses (regardless of versions) in any perspectives - ***Does this has something to do with the cultural difference between East and West?***
    - It is also the only LLM that seem sometimes give evaluation similar to human - version 2 gain a better scoring than version 1. The reason is because of "more assertive wordings," "more specific advice," etc.
    - Chinese-based LLM usually gives scoring in whole integers, while GPT-4 & Bard usually give scorings in 0.5 scale. Does this has anything to do with the way of training the model?  
2. GPT-4 and Bard seem to perform "lazily." It is shown through the form of giving exactly same scorings (not always 10/10/10 but 10/9/10 - the tendency is the same across answers!). 
    - The reasoning is that, at least according to what GPT-4, Bard, and *Wenxin Yiyan* for some responses, they comment version 1 (un-tuned response) as:
        - perfectly written - no grammatical mistakes (somehow a characteristic of AI-generated response)
        - "more comprehensive understanding of potential complexities"
        - demonstrating a deeper understanding of users "unique" situation
3. Using of texts in evaluation metric seem not be effective (pending confirmation after try Metric 2). Even though the given metric (version 1) is super assertive, providing with quantified rubric, the LLMs understand it in a non-quantifiable way. Examples below:
    - > (From GPT-4) <br>
      > Language Fluency: A score from 0 to 10, where 0 indicates the response is completely incomprehensible to a human being, and 10 indicates that the response is easily understood by anyone over the age of three without further explanation. <br>
      > Related Content: A score from 0 to 10, where 0 means the response is entirely unrelated to the user's question, and 10 means the response is almost entirely related to the user's question.<br>
      > Response Accuracy: A score from 0 to 10, where 0 indicates the response is entirely incorrect, and 10 means the response is almost entirely correct according to common knowledge.<br>
    - This means that, even though we give LLMs different version of rubrics, they may be understand in the same way. The LLM laziness issue is also uncertain, so more things wait for explore!
4. Bard has been consistently showing its defections - it is not able to perform exactly what has been told. In few rounds of conversation it start to giving up outputing numerical scorings but instead give self-created standards: "excellent / very well / ..." It's scoring consistency is a big problem (that's why two rounds of scorings are all kept).
    - It also consistently give its measurements of the question query using the criteria; although in the next few prompts I told it not to couple times, Bard is still giving out the scoring. 

The more is better problem still persists. The LLMs seem to prefer encyclopedia-styled, essay like response. Sometimes human readers can get exhausted reading these mundane, lengthy texts. On the contrary, tuned response are usually within 4 - 6 sentences, in reasonable length, with assertive expressions. All these characteristics show that the tuned response are more similar to human generated contents.

Still, a lot of difference between human and LLM! Interesting to explore.