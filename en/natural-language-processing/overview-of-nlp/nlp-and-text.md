# NLP and Text

Natural language is the way humans speak and write to communicate with one another. It includes all the features of spoken and written languages like English, Spanish, or Chinese. In artificial intelligence and machine learning, working with natural language is very important. Systems need to understand, process, and sometimes generate text. This is why this task is often called **Natural Language Processing (NLP)**.

To process natural language, it’s important to understand the different layers of how language works. These layers help machines analyze and understand text correctly.

### Graphemics: The Letters and Symbols

Graphemics is the study of written symbols. It focuses on the smallest parts of written language – the **graphemes**, which are the letters or characters used in writing. In English, these are the 26 letters from A to Z.

For example, in the word **“apple”**, the graphemes are: _a, p, p, l, e_.

Computers often work at this level first, especially when reading raw text data. Tasks like removing punctuation, handling capital letters, or identifying unusual characters are all related to graphemics.

### Morphology: The Structure of Words

Morphology is about the internal structure of words. It looks at **morphemes**, which are the smallest units of meaning in a word. Some words have only one morpheme (like “dog”), while others have several.

For example:

* **“unhappy”** = _un_ (a prefix meaning “not”) + _happy_
* **“cats”** = _cat_ + _s_ (a suffix meaning “more than one”)

Understanding morphology helps machines with tasks like stemming (reducing words to their base form) or identifying word variants.

### Syntax: The Structure of Sentences

Syntax deals with how words are arranged in a sentence. It follows rules that define how phrases and sentences should be constructed.

Example:

* **Correct syntax**: “The dog chased the cat.”
* **Incorrect syntax**: “Dog the chased cat the.”

Computers use syntax to figure out sentence patterns, detect grammar errors, and analyze sentence structure. Syntax trees or dependency parsing help break down how different parts of a sentence relate to one another.

### Semantics: The Meaning of Words and Sentences

Semantics is about meaning. It helps machines understand what words and sentences actually mean, not just how they are written or spoken.

Example:

* “He kicked the bucket.”\
  A human may know that this is a common **idiom** meaning “he died.”

But a machine without semantic understanding might think it means someone hit a metal container with their foot.

Semantics also includes things like:

* Understanding synonyms (e.g. “big” and “large”)
* Resolving ambiguity (“bank” as a river bank or financial bank)
* Matching questions with the right answers

Modern AI models like GPT and BERT use **semantic embeddings** to understand meanings better by converting text into vectors that capture meaning.

### Styling: The Tone and Form of Language

Styling refers to how language is used depending on context, emotion, or audience. Style can be formal or informal, polite or rude, technical or casual.

Example:

* **Formal**: “We regret to inform you that your application has been denied.”
* **Informal**: “Sorry, but you didn’t get in.”

For machines, detecting style is important in applications like chatbots, writing assistants, or translation systems. Changing the tone based on the user or the situation makes AI feel more natural and respectful.

### Pragmatics: The Use of Language in Context

Another important topic is **pragmatics**, which is about how people use language in real-life situations. Sometimes, the meaning of a sentence depends on the context.

Example:

* “Can you open the window?”

This is not a question about ability. It’s a polite **request**. Machines need to understand this type of hidden meaning when interacting with users.

#### Summary

To understand and use natural language, machines must go through several layers:

* **Graphemics**: the letters and symbols
* **Morphology**: how words are built
* **Syntax**: sentence structure
* **Semantics**: meaning
* **Styling**: tone and form
* **Pragmatics**: context and purpose

Each layer brings AI closer to communicating like humans. This process is complex, but with the help of modern models and data, machines are improving at understanding human language every day.
