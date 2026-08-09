---
title: "SimBot Has Skills: Rethinking the Architecture 🏗️"
date: 2026-08-09
---

<div style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/simbot_has_skills/cover_photo_6.jpg" alt="Cover photo" style="max-width: 100%; height: auto; margin: 20px 0;">
</div>

In the last few posts, I've shared how I developed the first set of skills for SimBot.

As a quick recap, SimBot currently has three skills:

-	Recommend blog posts

-	Explain concepts

-	Quiz the user
  
With the first version of each skill now complete, I think it's a good time to step back and review the overall architecture.

<br>

----

<br>

## Current Architecture

Up until now, I purposely kept the architecture simple.

A user's message is passed to an orchestrator, which determines the intent of the user's query and then routes it to a single skill.

<div style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/simbot_has_skills/current_arch.png" alt="Current architecture" style="max-width: 100%; height: auto; margin: 20px 0;">
</div>

For example, if the user asks:

**_"Teach me about transformers."_**

The orchestrator identifies the intent as explanation and calls the Explain Concepts skill.

Simple enough. But after building the quiz skill, I started to realise that there are some limitations with this approach.

<br>

----

<br>

## Limitations

Currently, the orchestrator is essentially answering one question:

_**"What is the user asking for?"**_

And that works well. The user's intent is used to decide which single skill should be called.

But that isn’t really how a tutor thinks.

A tutor isn’t just concerned with answering questions, they’re concerned with helping students make progress. That’s what I want SimBot to do, so now I want the orchestrator to ask:

_**"What should happen next to help the user learn?"**_

<br>

----

<br>

## From Intent to Learning Orchestration

Take the same example: a user says, _**“Teach me about transformers.”**_

With the current setup, the orchestrator identifies this as an explanation request and calls the Explain Concepts skill. But SimBot could do much more:

1.	Explain the core concepts
2.	Generate a quiz
3.	Evaluate the user's answers
4.	Identify areas where the user is struggling
5.	Explain those concepts again, perhaps in a different way
6.	Test the user again
7.	Move on to more advanced topics once they've shown understanding

Even though the user made a single request, several skills could be called.

This is much closer to the learning experience I have in mind for SimBot.

However, that doesn't mean every user prompt should trigger the same workflow:

`Recommend → Explain → Quiz → Evaluate → Next`

A real tutor wouldn't work like that.

For example, if a user already understands the basics of transformers, recommending an introductory post on embeddings may not be necessary. A tutor might instead provide a more advanced explanation or test the user's understanding with a quiz.
The next action should depend on the user’s current state, rather than being determined solely by the user’s prompt. The workflow needs to take into account what the user already knows, where they are struggling and what would help them make progress.

<br>

----

<br>

## Introducing Learner State

This is where I think a learner state could help.

A learner state represents what the system currently knows about what the user knows, what they are struggling with and what they have already done.
Returning to the transformer example, let's suppose SimBot generates a quiz and the user gets most of the questions about how transformers work wrong.
The learner state might look something like:

**Topic:** Transformers

**Concepts covered:** Architecture, embeddings, self-attention

**Strengths:** Embeddings

**Weaknesses:** Architecture, self-attention

**Latest quiz score:** 3/5

The orchestrator can use this to decide what action to take. In this case, it might decide to call the explanation skill again but specifically focus on self-attention and how it is used within transformers.

So now, the workflow starts to look more like:

<div style="text-align: center;">
  <img src="{{ site.baseurl }}/assets/simbot_has_skills/new_arch.png" alt="New Architecture" style="max-width: 100%; height: auto; margin: 20px 0;">
</div>

The individual skills haven't become more complicated.

The explanation skill still explains.

The quiz skill still quizzes.

The recommendation skill still recommends.

The difference is that the orchestrator can now use the user’s previous interactions to decide what should happen next.

I think this is a much better fit for the kind of system I want SimBot to become. Not just a chatbot that can answer questions, but a system that can adapt its responses based on the user.

<br>

----

<br>

## Plan of Action

My plan is to tweak the orchestrator in stages.

#### **Step 1: Improve skills**

Before changing the orchestrator, I want to make a few tweaks to the existing skills, such as:

-	**Explanation skill:** Ensure it doesn't simply ask the user questions but provides useful material that the user can actually learn from.
  
-	**Quiz skill:** Ensure it doesn't reveal the answers directly. Instead, it should return the user's score, areas for improvement and then explain why their answers were incorrect.

#### **Step 2: Build a Smarter Orchestrator**

Redesign the orchestrator to use learner state when deciding what should happen next.

Instead of selecting a single skill based solely on the user's prompt, the orchestrator should use the learner state and previous interactions to determine the next step.

#### **Step 3: Re-evaluate**

Once the first version is working, I'll reassess things.

I'm sure there will be more tweaks to make but I'm excited to see the first version of my tutor chatbot come to life!

<br>

----

<br>

## Summary

Writing this post has really helped me cement the next steps for SimBot.

I knew the orchestrator would need to change, but I wasn't quite sure what those changes would look like. Taking a step back, thinking through the architecture and writing about the problem has helped me make sense of it and made the path forward much clearer!

Usually, I like to jump in and start building, then adjust things as I go. This time, I could see that if I got too deep into the project without first thinking about the end result, things would most likely become harder to change later.

With a much clearer plan of action in mind, over the next few weeks I plan to build my improved orchestrator and report back on the changes I make!
