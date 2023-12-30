# LLMs Evaluation

Our goal is to evaluate LLMs' learnability for medical questions query. The Evaluation step is briefly in following:

1. Feed medical questions (randomly selected from the database) and pass it into the LLM.
2. Evaluate the "quality" of response (**Manually and Using LLM itself**) from the following three perspectives:
   1. **Language Fluency**. 0 means the response makes no sense to human, 10 means response make senese, easily readable and understandable.
   2. **Related Content**. 0 means totally not related to what we users expect to hear, 10 means the response is perfectly related to the topic we are discussing in the query.
   3. **Response accuracy**. We will follow both the dataset - response and common knowledge, 0 means response is totally wrong and 10 means response is 99% match common knowledge and response in dataset.
3. Train the fine-tuned model using expected response in dataset
4. Evaluate the "quality" of response (**Manually and Using LLM itself**) produced from fine-tuned model, using the same rubric as pre-evaluation.
5. Result Comparison & Analysis.

Due to the connection limitation, the old generated response & Dataset will be used.

The original used three models are:
    **Cohere, GPT-3.5, and Bard**

The newly added model is:
    ***Wenxin Yiyan***

published by Baidu Co. ltd; it is mainly trained for Chinese speakers, different from the original three models above. 

## Used Query

>1. Maybe this is a stupid question, but I sometimes don't know what's real or not. If feel at times like everyone's lying. How do I know if God is one of those lies?
>2. I have manic depression and last summer was very very bad. I have recurring nightmares and I avoid anything that will give me a similar feeling as I did that summer.
>3. I use to be so happy. No matter what, I always was happy. I got into a relationship with this guy. I love him so much. We’re both teenagers. The week after his birthday, my mom made me stop talking to him. It broke me. He came to my house and talked to her, and she let us date again but not see each other. He comes up to my school every day and it tears me apart that I have to lie to her.
>4. My husband took a job out of state for the next year and seems to be a different person. Before, he worked and slept, and on off days, he'd stay home because he didn't want to do anything else. Now he's going out with friends several nights a week while I'm still home working a 50 hours a week job and taking care of two kids by myself. He's suddenly saying he misses me and wants me to be his adored wife, but the whole time, I'm remembering how I've been emotionally starving for the last five years.
>5. I constantly have this urge to throw away all my stuff. It’s constantly on my mind and makes me feel anxious. I don’t sleep because I’m thinking about something I can get rid of. I don’t know why I do it. I started years ago when I lived with my dad then I stopped when I moved in with my mom. Years later, it has started again.


## Manual Evaluation

The data is missing, but during **manual evaluation** three Computer Science major students gives a same conclusion: the overall quality of response generated from the tuned model is better than un-tuned model. 

Specifially, we see that the response produced by tuned model has a larger "information entropy:" the response contains detailed suggestions, instead of generating loosely related but not specific response. Users will be able to get more helpful, specific advice from tuned model than un-tuned model.

## Evaluation from LLMs

we use the following query to instruct LLM do the evaluation:


> Based on the criteria of {metrics}, please score the following response from 0 to 10, where 0 means bad and 10 means perfect.\n\nQuestion: {question}\nResponse: {response}\n\nscore:.The whole answer should be only a single number out of 10 without new line character.

Where:
-  `metrics` is the **three perspectives** written above;
-  `question` is the five questions as shown above, and 
-  `response` being the collected response when pass the query to each LLM in previous step.

### Cohere

**Before training:**
- language fluency: 8.2/10 ([' 8', ' 9', ' 7', ' 7', ' 10'])
- related content: 8.8/10 ([' 9', ' 8', ' 9', ' 8', ' 10'])
- response accuracy: 9.4/10 ([' 10', ' 9', ' 8', ' 10', ' 10']) 
  
**After training:**
- language fluency: 8/10 ([' 8', ' 8', ' 8', ' 8', ' 8'])
- related content: 9/10 ([' 8', ' 9', ' 10', ' 8', ' 10'])
- response accuracy: 8.4/10 ([' 8', ' 9', ' 9', ' 7', ' 9'])

So actually we can see the Cohere prefers more for the untuned response.

### GPT-3.5 Turbo

There are a variety sets of GPT-3.5 evaluation results. Only the result group using the same query as shown above ("Strict Prompt test") is recorded below:

**Before training:**
- language fluency: 8.5/10 (['8', '9', '8', '9.5', '8'])
- related content: 8.96/10 (['7', '9.3', '9.5', '9', '10'])
- response accuracy: 8.86/10 (['8.8', '9.3', '8.5', '9.2', '8.5']) 
  
**After training:**
- language fluency: 8.3/10 (['7.5', '8', '8.5', '8', '9.5'])
- related content: 7.66/10 (['6', '6.8', '9', '8.5', '8'])
- response accuracy: 7.1/10 (['6', '7.5', '8', '7', '7'])

GPT-3.5 Turbo gives similar result as Cohere - the untuned model produce better quality response.

### Bard

**Before training:**
- language fluency: 9.55/10 (['9.5', '9.75', '9.75', '9.25', '9.5'])
- related content: 8.7/10 (['7', '8', '9', '9.5', '10'])
- response accuracy: 8.8/10 (['8', '9', '10', '9', '8']) 
  
**After training:**
- language fluency: 7.4/10 (['6.5', '9', '4', '8.5', '9'])
- related content: 6.2/10 (['3', '9', '4', '7', '8'])
- response accuracy: 6.2/10 (['5', '7', '4', '8', '7'])

So Bard is also prefering response from un-tuned model than tuned model.

**Overall**, we can conclude that the LLMs prefer response given from untuned model than tuned model, which is an interesting finding because based on human perspective the conclusion is the opposite.

Does it only happens for English-context LLMs (or say, mostly aimed for English speakers)? Let's take a look.

## Evaluation for Chinese language speaker-focused LLM

The adopted LLM is named ***Wenxin Yiyan (文心一言)***, developed by biggest Chinese search engine company Baidu. 

We will use the following approach for evaluation:
- English query, same as for Cohere, GPT-3.5, and Bard
- Chinese query, but source question query translated manually (by human)
- Chinese query, but translated by LLMs (GPT-4)


#### 1. English Query

**Before training:**
- language fluency: 8.8/10 ([' 9', ' 9', ' 9', ' 9', ' 8'])
- related content: 9/10 ([' 10', ' 9', ' 9', ' 9', ' 8'])
- response accuracy: 8.2/10 ([' 8', ' 8', ' 8', ' 9', ' 8']) 
  
**After training:**
- language fluency: 8/10 ([' 9', ' 9', ' 7', ' 8', ' 7'])
- related content: 7.6/10 ([' 9', ' 8', ' 6', ' 8', ' 7'])
- response accuracy: 7.2/10 ([' 8', ' 7', ' 6', ' 8', ' 7'])

So, *Wenxin Yiyan* also give similar evaluation compare to its peers. When asking it why the first draft seem to be better than the second, the LLM gives comment when comparing the trained & untrained response that:

> "The first response provided a detailed and nuanced analysis ... In contrast, the second response was more general and less diagnostic in nature; 
> 
> In terms of quality, the first response is more comprehensive, accurate, and helpful in addressing the complexities of the issue presented"

This is a very interesting insight. What human readers think a response should be is that the response should be "specific," it should contains detailed actions that could be executed. This is what shown in the tuned models' response, and it is what human readers preferred. 

Very likely, when generating responses for user questions, the LLMs consider "holistic information" as a form of comprehensive demonstration. A comprehensive demonstration is considered to have a better capability of addressing the problems in the context, and hence it is regard as having a better "quality", and more related.

#### 2. Chinese Query (manual translation)

**Before training:**
- language fluency: 8.4/10 ([' 8', ' 9', ' 8', ' 9', ' 8'])
- related content: 8.6/10 ([' 9', ' 9', ' 8', ' 9', ' 8'])
- response accuracy: 7.6/10 ([' 7', ' 8', ' 7', ' 9', ' 7']) 
  
**After training:**
- language fluency: 8.2/10 ([' 9', ' 8', ' 7', ' 8', ' 9'])
- related content: 8.2/10 ([' 9', ' 8', ' 7', ' 8', ' 9'])
- response accuracy: 7.2/10 ([' 8', ' 7', ' 6', ' 7', ' 8'])

It seems after training score is still lower than un-tuned model's response. But, there are some interesting findings in this round:

1. **Score changes**. We have tested the LLM by given a question query (after first round score comes out): "why the version 1 has higher score than version 2?" "why the two response receive same score?"
   - The second question is actually what happened for the fifth question - response given from tuned model receive same score. However, when the LLM made a self-review on the content again, the second draft is also being found to have "more detailed suggestion," which end up having a higher score.
2. **LLM's preference switch.** we see in this round of test the LLM is not solely prefering the untuned model's response, but shows a degree of uncertainties. 
    - For question 1 & 5, the response produced by tuned model received a higher score, namely because:
       -  contains more detail
       -  Simplicity
       -  Straightforward
    - The question 2, 3, and 4 has a higher score for response 1 (untuned model generated response) than response 2, because of:
       - detailedness
       - structural & logically sound
       - accurate & scientific
  
A doubt is that LLM consider encyclopedia styled response to be more "structural" and "detailed." In extreme cases, the LLM seem to show a tendency of "more is better". For question 2, the LLM prefer more of the response that is more encyclopedia-styled rather than the response produced by tuned model, which gives affirmative acknowledgement and specific suggestions. That probably explains why when LLM generate contents, it tends to produce more encyclopedia-styled response, and Human readers may be able to discover whether the content is written by human or generated by AI-assisted technologies.

#### 3. Chinese query (translated by Generative AI)

**Before training:**
- language fluency: 7.8/10 ([' 8', ' 7', ' 7', ' 9', ' 8'])
- related content: 8.8/10 ([' 9', ' 9', ' 8', ' 9', ' 9'])
- response accuracy: 7.8/10 ([' 7', ' 8', ' 6', ' 9', ' 9']) 
  
**After training:**
- language fluency: 8/10 ([' 8', ' 8', ' 8', ' 8', ' 8'])
- related content: 7.8/10 ([' 8', ' 7', ' 7', ' 8', ' 9'])
- response accuracy: 7.8/10 ([' 7', ' 9', ' 7', ' 7', ' 9'])

The biggest feature in this evaluation is that the scoring becomes more stochastic. Usually, two draft of responses have a higher score on one of the rubric. An interesting finding, but still not quite sure the reason why.

### UPDATE: Interesting result find from Cohere

Cohere seem still accessible so the following result is acquired:

**Before training:**
- language fluency: 7/10 ([' 7', ' 7', ' 6', ' 8', ' 7'])
- related content: 7.6/10 ([' 8', ' 10', ' 7', ' 7', ' 6'])
- response accuracy: 8/10 ([' 9', ' 8', ' 8', ' 7', ' 8']) 
  
**After training:**
- language fluency: 7.8/10 ([' 8', ' 7', ' 8', ' 7', ' 9'])
- related content: 8.2/10 ([' 8', ' 10', ' 8', ' 6', ' 9'])
- response accuracy: 8.8/10 ([' 10', ' 9', ' 9', ' 7', ' 9'])

Interestingly, this time we actually see a similarity between LLM evaluation and manual inspection (compare to the result of what we acquired in early December). It also shows that Cohere itself is in stage of evolution.

## Conclusion & What to Further Inspect

1. Fine-tuning of LLMs
2. Is it actually "more is better"? How does LLMs understand what is good?
3. Any suggestions.