---
layout: post
title:  "Building a Mandarin Learning App with AI - part 1"
date:   2025-07-10 15:32:04 -0400
categories: ai mandarin claude
---
As AI tools have taken off, I've been spending a lot of my free time using them to create some greenfield side project apps. One of those  projects has been a web app for studying Mandarin. 

## A very long back story..
I started studying Mandarin back in 2017 before we had kids. First, I tried Fluent City which was..not great. Here's an example of the sentences we had to translate for our homework. Somehow these phrases have not been helpful in my life.

![Fluent City taught me useless phrases](/assets/images/2025-07-10/fluent_city.jpg)

Thankfully, I switched to [China Institute](https://chinainstitute.org/) which was amazing. At the time, I was working at Aaptiv in 1 WTC so it was a convenient 10-minute walk to get there. Even with the close proximity though, towards the end of my pregnancy in late 2018, I abandoned my studies because I was so tired and uncomfortable from the pregnancy. While I continued to dabble in [some apps](https://www.duolingo.com/) to practice vocabulary, I largely put my studying aside. 

Fast forward to this spring, I finally felt like I was in a place where I could sign back up for a class. Thankfully China Institute offers virtual classes so I would just need 2 hours/week for class, plus additional time to study and complete homework. Given I hadn't really been studying, I decided to start back off at level 101 which was a breeze. Level 102 was mostly review as well, but level 103 started getting a little trickier as we really switched to using characters instead of [pinyin](https://en.wikipedia.org/wiki/Pinyin). I needed a way to study the _specific_ vocabulary I had to learn for class and none of the apps out there let me easily tailor the word list to my needs. I decided this would be a great opportunity to use AI to create a Mandarin review app tailored to my needs.

## The approach
Given this was a pretty straightforward use case, I decided to start my efforts using LLM chatbots that can produce code (think Gemini canvas or Claude artifacts) instead of trying to build it with AI autocompletion or agentic AI coding tools. 

I also decided that given there was little effort required to get started, I would try a couple of options. I settled on trying with Claude and Gemini. 

## Claude
My first attempt was with Claude Sonnet 4. I decided to quickly write a single prompt containing the skeleton of what I wanted and then see how it went.

Here's the initial prompt I included:
![Create a flashcard web app for learning Mandarin Chinese using simplified characters. The app should use Typescript, React.js, and Node.js. The app should do the following:
From the given list of words/phrases, present a random single Mandarin word/phrase at a time.  It should initially show only the simplified Chinese character for the word.
There should be an input where the user can enter the English meaning for the word or phrase.
There should also be a button to give a hint. If the user presses the hint button, the app will now display the pinyin version of the word in addition to the simplified Chinese character for that word.
There should be a "I don't know" button. If the user taps the button, the app will show the English meaning for the word, as well as the pinyin for the word if it is not yet displayed before displaying the next word/phrase flashcard.
The app should continue going until it has run through every single word/phrase it has. Once all words have been reviewed, it should show the total number of words reviewed, how many words the user got correct without a hint, how many words the user got correct with the pinyin hint, and how many words the user didn't know. A list of the words that the user did not know should be presented including the simplified character, the pinyin, and the English meaning of the word/phrase.](/assets/images/2025-07-10/claude_prompt.png)

Claude diligently got to work and decided to build out the frontend and then the backend, as well as provide a list of the configuration files and steps on how to run the app. 

### How did it do?

#### Frontend 
Claude did use the technologies I asked for (hooray!) namely React and Typescript. That said, it also decided to add Tailwind CSS to the mix. It's a reasonable choice, but it just sort of snuck it in there. 

The React code it produced is all intelligible, if maybe a bit clunky. It also made the wise decision to use `toLowerCase()` and `trim()` when doing the string comparison to see if the answer was correct.

There _was_ one thing it missed which we'll talk about in a sec..

#### Backend 
Again, Claude used the technologies I asked for- in this case, Node and Typescript. However, this time it did not introduce its own technical decisions and decided to create a hardcoded list of Chinese words in the Node server code.
![Node server code with hardcoded Chinese characters](/assets/images/2025-07-10/claude_backend.png)

This is very likely not how someone would want to build this for real™, but skipping a database at this point makes experimenting a lot easier so it was a blessing in disguise. 

#### Running the app
Claude gave these instructions for running the app which looked solid:
![Instruction for running flashcard app](/assets/images/2025-07-10/claude_instructions.png)
..but upon hitting the final step of `npm run dev`, I noticed a *big* oversight:
```
Could not find a required file.
  Name: index.html
```
When I flagged the error to Claude, it realized it had also forgot to include the React entry point. Whoops.
![Claude realized its mistakes](/assets/images/2025-07-10/claude_fix.png)

After adding all of the additional files, the app worked! I did have to ask Claude to make a few modifications:
* clearing the input after we move to the next flashcard 
* only showing the hint if the user clicks on the hint button for the specific card they are on. 

![Final version of Mandarin flashcard app](/assets/images/2025-07-10/final_app.png)

You can find the source code [here](https://github.com/kathleen/mandarin-flashcards). It's been left mostly as Claude created it besides me adding some more Chinese words.  

## Takeaways
Hands down, the hardest part of this experience is writing the correct prompts to get what I want. Having to sit down and articulate all of the requirement details is tiresome; if I don't do it, it's a gamble whether Claude will fill in the blanks properly itself. Next time I will try to use AI to write a draft of the requirements for me.

Also, this specific breed of tool would likely be challenging for someone with no engineering experience to use. It didn't explain how to get setup with Node, it didn't explain how to run things in the terminal, etc. 

Stay tuned for how Gemini did.
