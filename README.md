# TakeMeter — Project 3

A fine-tune text classifier that evaluates discourse quality in the r/PokemonUnbound SubReddit Community.

---

## Community
The Community that I’ve chosen is r/PokemonUnbound, a SubReddit dedicated to the Pokémon Unbound Rom Hack. This Community is a Good Fit for a Classification Task because Users are able to create different types of Posts, including requesting help, showcasing their achievements, team recommendations, and any general discussions about the Game itself. Since Users interact with the Community for different purposes, the discourse is varied enough to make the classification meaningful and interesting.

---

## Annotated Dataset
See r_PokemonUnbound_Posts_Data

### Difficult Labels
Example 1:
Post: "Finnaly finished my first run of Pokemon Unbound. This is one of the “original” rom hacks I really enjoyed. Second to Heart & Soul. What do you think my team says about me?"
Decision: achievements
Reasoning: Despite te User is asking others of what they think about him based on his team, inviting a conversation to happen, the purpose of this post isthat the User is showcasing their first completion on beating the Elite Four.

Example 2: 
Post: "Must haves for the game. Tell me ur must have mons for your team in this game, I'm 4 badges into this game and I have enjoyed it but I'm struggling to find a team that doesn't struggle."
Decision: team_building
Reasoning: Despite the User is asking other's of their experiences, the main purpose of this post is to ask any pokemon recommendations.

Example 3: 
Post: "Quick trade, need a scyther. Anybody got a Scyther they could trade real quick?"
Decision: discussion
Reasoning: Despite the fact that the User is asking for an assistance, in which this case, a trade, this post doesn't generally ask needing help regarding the game mechanics. It is more like a deal with other users.

---

### Labels
help_request: The Primary Purpose of the Post is to seek Assistance, Advice, Clarifications or Solutions related to the Gameplay, Progression, Technical Issues or Game Features.
     - “Where do I find Riolu in this game?”
     - “I’m having trouble downloading the game.” 

achievements: The Primary Purpose of the Post is to share a User’s of their Personal Accomplishments, Challenge Completion, Hall of Fame Team, Shiny Encounter, Milestone or other achievements.
     - "Finally beat the Elite Four with my Mono Fire Run!”
     - “After 15,357 encounters, I finally found a Shiny Riolu!”

discussion: The Primary Purpose of the Post is to present an Opinion, Comparison, Question, or an Idea intended to generate a Conversation among the Community Members.
     - “I created a Pokemon Unbound Sequel/Remake.”
     - “Do you guys think Unbound is better than Radical Red?”

team_building: The Primary Purpose of the Post is to Discuss, Evaluate, Recommend, or Optimize Pokemon Teams, Movesets, Team Composition, Synergy or Battle Strategy.
     - “Rate my team before I challenge the Elite Four.”
     - “I’m running an ice type monotype run, what ice pokemons are good?”

---

## Label Distribution
help_request 	  → 	60	→ 	30%
achievements 	  → 	55	→ 	27.5%
team_building 	→ 	44	→ 	22%
discussion 		  → 	41	→ 	20.5%

-- 

## Evaluation Report of Models

### Overall Accuracy
Model                               			Accuracy
---------------------------------------------------
Zero-shot baseline (Groq)              	  0.633
Fine-tuned DistilBERT                  		0.300
---------------------------------------------------

### Per-class Metrics
[Baseline (Groq)]
[Labels]          [Precision]  [Recall]  [F1-Score]  [Support]
help_request         0.53		     0.89		    0.67		     9
achievements         0.88		     0.78		    0.82		     9
discussion           0.33      	 0.17      	0.22         6
team_building        0.75      	 0.50		    0.60         6

[Fine-Tuned DistilBERT]
[Labels]          [Precision]  [Recall]  [F1-Score]  [Support]
help_request         0.31		     1.00		    0.47		     9
achievements         0.00		     0.00		    0.00		     9
discussion           0.00      	 0.00      	0.00         6
team_building        0.00      	 0.00		    0.00         6

### Fine-Tuned Model Confusion Matrix
[True/Predicted Label]      [help_request]    [achievements]    [discussion]    [team_building]
help_request                      9                 0                0                 0
achievements                      8                 0                0                 1
discussion                        6                 0                0                 0
team_building                     6                 0                0                 0

### Fine-Tune Model's Failure Analysis (3 Examples) 
[Example 1] 
Text: “Ran it back on Expert. Team of mid + metagross, the difficulty difference between insane and expert is wild.”
True Label: achievements
Predicted Label: help_request
Analysis: The text is talking about completing an elite four run, which classified the text with an ‘achievements’ label. However, the model instead predicted it as ‘help_request’. A reason why this is happening would be how the model seems to focus on any keywords related to the gameplay, rather than recognizing that the user is showcasing their accomplishment.

[Example 2] 
Text: Pokemon suggestions for elite 4. What are some good tanks /special attackers I could bring to the e4?”
True Label: team_building
Predicted Label: help_request
Analysis: The text is focusing on team composition and pokemon recommendations regarding beating the elite four, which classified the text as a ‘team_building’. However, the model instead predicted it as ‘help_request’. A reason why this is happening would be because the text is asking a question which satisfies both labels. This is showing that the model is struggling in different ‘help_request’ with ‘team_building’.

[Example 3] 
Text: “Entei or Charizard ? (Difficult / Insane). I’m really struggling to understand how some Pokémon are stronger or weaker than others based on the information given in the game and in Pokémon in general....”
True Label: discussion
Predicted Label: help_request
Analysis: The text is asking for an opinion regarding two pokemons, which classified the test as a ‘discussion’ by how it intended to generate a discussion. However, the model instead predicted it as ‘help_request’. A reason why this is happening would be how the model would treat most of the questions as a request or assistance. This is showing the the model did not learn how to distinguish the text sentences.

### Labels Most Frequently Confused
The most frequent confusion was the false labeling with [achievements], [team_building], and [discussion], being predicted as [help_request] instead. Because the Fine-Tuned Model  rarely predicted the first three labels, and instead predicted [help_request] for most of the posts, it indicated that the Fine-Tuned Model had failed to learn distinguishing between the four labels.

### Why These Boundaries Are Difficult
One reason would be how several of the posts label, involves the user asking questions like how [help_request] posts involve users asking for assistance regarding the game mechanics, and [team_building] asking for recommendations regarding pokemons. Another reason would be how all of the labels posts besides [help_request] posts, contain language that is question-like. This makes the model struggle to learn the difference between them.

### Labeling Problem or Prompt/Data Problem?
Based on the evidence provided, it would seem to be more of a prompt/data problem. The reason why is because the dataset given, was relatively balanced across all of the labels, and the labeling rules were consistently applied throughout the annotation. The Confusion Matrix also shows that the classifier collapsed toward one dominant prediction, that being [help_request]. This is suggesting that the model failed to learn useful decision boundaries.

### Improvement/Fixes
One improvement would be including the images alongside the post text. Many r/PokemonUnbound Posts include screenshots of teams, Hall of Fame teams, shiny encounters, or achievements that provide important context. Another improvement would be collecting additional label examples, particularly [discussion] and [team_building], so that the model can better learn the differences between these categories.

---

## Common Themes in Wrong Prediction
- Over-prediction with the [help_request] label.
- Team-building posts with help-like languages.
- Confusions between discussion vs help_request posts labels.
- Post combining achievements with questions.
- Post containing multiple intent, causing confusion.
- Low confidence predictions everywhere.

---

## Sample Classifications
| Post | Predicted Label | Confidence Level |
|------|-----------------|------------------|
| Struggling on Vanilla difficulty 🤦🏽‍♀️. So embarrassing, but I can't beat the last fight in the mansion 🤦🏽‍♀️ what Pokémon should I use please? | help_request | 0.28 |
| Pokemon suggestions for elite 4. What are some good tanks /special attackers I could bring to the e4? | help_request | 0.27 |
| YAYY I did it. Was a lot easier than I remembered, thanks Pokemon Showdown | help_request | 0.26 |

---

## Reflection

### What the Model Captured
The Fine-Tuned Model primarily learned the patterns of [help_request] posts. The Model was very sensitive to questions based like languages, gameplay related terminologies, and asking for advice. Due to this, the Model predicted posts as [help_request] but relied too heavily on one label, rather than fully using the other labels.

### What the Model Missed
The Fine-Tuned Model failed to learn how to distinguish between [achievements], [team_building], and [discussion] labeled posts. Many of the posts contain question-based languages that relate to [help_request] labels. Instead of fully learning the true purpose of the posts, the Model just often relied on surface leveled keywords, which the classified the examples as [help_request].

---

## Spec Reflection

### How Spec Helped
The Project Specs helped guide my implementation by how it made me carefully create labels and declare the labeling with precision and understanding. When defining the labels in my planning.md file, it helped with the labeling of the post by how it lets me understand and distinguish the posts. Also how it wants me to analyze where it fails, helped me discover how the Fine-Tuned Model of mine struggled in certain parts and such.

### Implementation Diverges
One way my implementation diverges would be how I initially wanted a classifier to reliably distinguish posts between all of the four labels. However, after the Fine-Tuning, the Model collapsed towards one dominant prediction, that being [help_request]. This is why my evaluation focuses on analyzing where and why the Fine-Tuned Model failed.

---

## AI Usage

<!-- Describe at least 2 specific instances where you used an AI tool during this project.
     For each: what did you give the AI as input, what did it produce, and what did you
     change, override, or direct differently?

     "I used Claude to help me code" is not sufficient.
     "I gave Claude my Chunking Strategy section from planning.md and asked it to implement
     chunk_text(). It returned a function using a fixed character split. I overrode the
     chunk size from 500 to 200 because my documents are short reviews, not long guides." -->

**Instance 1**

- *What I gave the AI:*
- *What it produced:*
- *What I changed or overrode:*
During this project, I have mostly used ChatGPT because Claude was having troubles in my case. I gave ChatGPT the outputs of my Colab Notebook because I kept having trouble understanding the outputs. ChatGPT then produced a detailed explanatory response explaining the outputs of my Colab Notebook. I keep asking it more questions for more clarifications until I understand it and write it on a google document before reviewing it and explaining it on my Github.

**Instance 2**

- *What I gave the AI:*
- *What it produced:*
- *What I changed or overrode:*
During this project, I have mostly used ChatGPT because Claude was having troubles in my case. I gave ChatGPT the watches of the Posts I gathered from r/PokemonUnbound, and asked it to evaluate the labels I have pre-planned. ChatGPT then returned suggested labels for the posts and even identified a few of the posts that appeared to be ambiguous or mislabeled. I then manually reviewed every of the suggested labels and made my final labeling decisions. In several cases, I kept some of my original labels when I believed they better matched the definitions provided on my planning.md.
