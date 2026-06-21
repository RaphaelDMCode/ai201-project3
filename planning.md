# TakeMeter — planning.md

> Write this document before you write any pipeline code.
> Your spec and architecture diagram are what you'll use to direct AI tools (Claude, Copilot, etc.) to generate your implementation — the more specific they are, the more useful the generated code will be.
> Update the Retrieval Approach and Chunking Strategy sections if you change your approach during implementation.
> Update this file before starting any stretch features.

---

## Questions

### Community
<!-- What community did you choose and why?
     Why is this community a good fit for a classification task — what makes the discourse varied enough to be interesting? -->
The Community that I’ve chosen is r/PokemonUnbound, a SubReddit dedicated to the Pokémon Unbound Rom Hack. This Community is a Good Fit for a Classification Task because Users are able to create different types of Posts, including requesting help, showcasing their achievements, team recommendations, and any general discussions about the Game itself. Since Users interact with the Community for different purposes, the discourse is varied enough to make the classification meaningful and interesting.

---

### Labels
<!-- What are your 2–4 labels?
     Define each in a complete sentence.
     Include 2 example posts per label. -->
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

### Hard Edge Cases
<!-- What type of post will be genuinely ambiguous between two labels?
     How will you handle it when you encounter it during annotation? -->
A Common Ambiguous Case occurs between the help_request and team_building labels. An example of this case would be when a User may ask for help, while also requesting recommendations for their team. To handle these cases consistently, I will assign the label based on the Post’s primary purpose. If the purpose was solving a Gameplay problem, I will label it as help_request. Else if the purpose was to evaluate or improve a Pokemon team, I will then label it as team_building.

---

### Data Collection Plan
<!-- Where will you collect examples?
     How many per label?
     What will you do if a label is underrepresented after 200 examples? -->
I will collect the Posts from the r/PokemonUnbound SubReddit. My Goal is to gather at least 200 Posts, with about 50 Posts per label to maintain a balanced dataset, then put it into a CSV file. If one of the labels is Underrepresented after collecting 200 Posts, I will continue on collecting additional posts from that category, until the dataset is more balanced.

---

### Evaluation Metrics
<!-- Which metrics will you use to evaluate your model and why are those the right ones for this specific task?
     (Accuracy alone is not enough — explain what else you need and why.) -->
The Metrics that I will be using to evaluate the classifier would be Accuracy, Precision, Recall, F1 Score and a Confusion Matrix. Accuracy measures the overall percentage of the correctly classified Posts, but it doesn’t reveal whether the Model performs well across all the labels, hence why Accuracy alone isn’t enough. That is why, Precision, Recall and F1 Score will be calculated for each label to evaluate how effectively the Model distinguishes between help_request, achievements, discussion, and team-building Posts. I will also be using Confusion Matrix to identify which labels are most frequently confused with one another, particularly help_request and team_building.

---

### Definition of Success
<!-- What performance would make this classifier genuinely useful?
     What would you accept as "good enough" for deployment in a real community tool? -->
The type of performance that I would consider the classifier to be successful is if it achieves an F1 Score of at least 0.70 for each of the labels. This Level of Performance would indicate that the Model can be reliably distinguished between the help_request, achievements, discussion, and team-building Posts. For a Real Community Tool, I would consider the Model “good enough” if it correctly classifies the Majority of the Posts while making occasional Mistakes on Ambiguous Cases.

---

## AI Tool Plan

### Label Stress-Testing
<!-- Give the AI your label definitions and edge case description, 
     and ask it to generate 5–10 posts that sit at the boundary between two labels.
     If it produces posts you can't classify cleanly, your definitions need tightening — do that now, before you annotate 200 examples. -->
[Boundary: [help_request] vs [team_building]]
"I've been stuck on the Elite Four for two days. Here's my current team: Garchomp, Lucario, Crobat, Rotom-Wash, Snorlax, and Arcanine. What changes would you make?"
Ambiguous Reasoning: The User is asking for Help, but the Help is specifically about improving a Team.
Recommended Label: [team_building]
Recommended Reasoning: Primary Purpose is evaluating and improving a Team.

[Boundary: [achievements] vs [discussion]]
"I finally completed an Expert Mode Nuzlocke after three failed attempts. Honestly, I think Unbound's difficulty is harder than Radical Red. What do you guys think?"
Ambiguous Reasoning: Starts as an Achievement, Ends as a Discussion.
Recommended Label: [discussion]
Recommended Reasoning: The Post invites Conversation and asks for Opinions.

[Boundary: [achievements] vs [team_building]]
"Finally beat the Elite Four with this Mono-Ice team! Froslass was definitely the MVP. What changes would you make if I wanted to use this team in Expert Mode?"
Ambiguous Reasoning: The User is sharing an Accomplishment but also requesting Team Advice.
Recommended Label: [team_building]
Recommended Reasoning: The Final Goal of the Post is Team improvement.

[Boundary: [discussion] vs [help_request]]
"Why do people say Aegislash is one of the best Pokémon in Unbound? Am I missing something?"
Ambiguous Reasoning: Could be asking for Information or starting a Discussion.
Recommended Label: [help_request]
Recommended Reasoning: The User is Primarily Seeking Clarification.

[Boundary: [discussion] vs [team_building]]
"Do you think every balanced team needs a dedicated tank, or is offensive pressure enough?"
Ambiguous Reasoning: Team-Building Topic, but framed as a General Discussion.
Recommended Label: [discussion]
Recommended Reasoning: The Goal is exchanging opinions, not Improving a specific Team.

---

### Annotation Assistance
<!-- Decide whether you'll use an LLM to pre-label a batch of examples before reviewing them yourself.
     If yes, note which tool you'll use and how you'll track which examples were pre-labeled (for disclosure in your AI usage section). -->
I plan to use both ChatGPT and Claude to assist with the labeling. After providing the Model with my label definitions, I will then ask it to generate preliminary labels for batches of unlabeled posts. I will then manually review every suggested and correct any mistakes before adding the examples to the database. To ensure transparency, I will keep track of which of the examples were initially pre-labeled by an AI Tool and which were labeled entirely by hand.

---

### Failure Analysis
<!-- Plan to give your list of wrong predictions to an AI tool and ask it to identify patterns before you write up your evaluation.
     Note what you'll look for and how you'll verify the patterns yourself. -->
<!-- I got confused on these Parts -->
After evaluating the classifier, I will then collect the posts that were incorrectly classified, then provide them to an LLM. I will then ask the Tool to identify the Patterns of the Mistakes, like whether certain labels are frequently confused with one another or with Ambiguous wordings causing Classification Errors. I will then review the Posts to verify the suggested Patterns and determine whether the Issue comes from Unclear Label Definitions, Inconsistent Annotations, or Limitations in the Training Data.

---
