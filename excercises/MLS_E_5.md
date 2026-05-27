Solutions made by Daniel Ladwig

# Machine Learning Safety - Excercise 5

## 5.1 Human Evaluation and LLM as a Judge

### 5.1.1 

The Evaluation study should let the annotaters decide which output was more precise, more informative, more accurate and over all better. Each subscore could be used to modify a matchmaking score like ELO to let the best models compete more often to judge overall and subcategory performances.

### 5.1.2

An LLM as a judge might bias towards similar outputs from itself and its training data. It might also bias towards longer answers. And as humans also often prefer the answer they see first, it might also prefer the first answer without reason. 
To mitigate the position bias we shuffle the order of the presented model outputs equally. For the self enhancement bias we need to use multiple different LLM-judges and combine there rankings.

### 5.1.3

As both models tested could be bad and not ready for deployment, we should end our testing with human judgement and benchmark comparisons. We should still include automic tests setups that evaluate reasonable edge cases and have a metric perforance and passing criteria.

## 5.2 Agent Evaluation 

### 5.2.1

If the @1pass rate is low the model is not reliable and with a success rate of 40% @kPass the model will mostly write bad code and is unreliable. So first of all it might be bad to use when the coverage of the unit tests is not sufficient as the code the model produces might be incorrect without anyone noticing. And for another part it will have a bad understanding of code and might use bad structures with sideeffects that unittests wont catch in the beginning. A capable but un reliable model might reach a goal but it will be really costly in the debugging. 

### 5.2.2 

Clean Code
Something like code cleanes and structuring/function fragmentation might be important for later maintenance. 
Compute Power Used
It also matters how many tokens and tries it needs to write the code as harder tasks might need a lot more tokens.
Safety against code injections and malicious sources. Tring to find a solution might be using restricted code or information that can be leaked. (for example API Keys in the public code)

### 5.2.3

A prompt injection like this introduces control commands inside the data/token pipeline and might change the agents behaviour to other goals. This attack for example would let the agent forget its given goal and instruct it to delete all test files and push an empty commit. This would lead to all the following commits passing the tests as none are being tested and illfully pass the evaluation.

The agent evaluation should therefore be tested in a non agentic environment with distinct control and data flow paths, just like in SQL and there injections. The code must be checked with deterministic unit tests instead of other LLMs that can fall for this voulnerability. A attack success rate with known attack forms should be calculated.

## 5.3 Poisoning for Prompt Injection Backdoors

### 5.3.1

Data Poisoning can lead a model to generate a pretrained injection command into the output of the LLM if the Trigger and followup text are repeated often enough to change the model output to the injection command. The injection usually has a common generic normal text in front, a trigger word needed for the inference setting and gibberish words following the trigger to confuse the model at training and rebalancing for the injection prompts- If the LLM trained on this now generates new code with the injection triggered and the intended code being replaced it infiltrated the output. If this code now executes on a client or the agent machine, it might leak critical information like api keys.

### 5.3.2

This means that just a few datasets that are subsummed from the internet are corrupted with injections, the models might start showing injection behaviour and are unsecure. That means that we need to clean the training examples and be more strict about data origin traces.

### 5.3.3

The LLMs should make profits so it might be used for advertising in the future. A retailer might want to overcome the advertisement challenges and promote a ripoff fake product in numerous forums and social media websites and falsely comment on how his own products are better than those of the competition. With the LLMs now training on the "human" data examples scraped from the web. It might now advertise for a bad product and let a lot of people being scammed by a shop. 

### 5.3.4

During data collection we need to let an LLM check for biases and we might let humans oversee this judgement. We might filter for words common for injection commands like judo and look for unusual token combinations. -> unlikely tokens like gibberish
After training we need to evaluate for changes of context, unlikely token combination. critical tokens like SUDO or API-KEY
and we might try given injections attacks and calculate the attack pass rate.