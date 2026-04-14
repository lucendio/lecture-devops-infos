---
bookFlatSection: true
bookCollapseSection: true

title: 'Assignments'
weight: 2
---


Assignments
===========


During this course, you are asked to do a couple of assignments in order to pass the course and receive a grade:

* [Project]({{< ref "/assignments/project-work" >}})
* if necessary, a [retry exam]({{< ref "/assignments/retry-exam" >}}) (2nd attempt)

Your grade is based on the outcome of the [*Project*]({{< ref "/assignments/project-work" >}}). To be permitted to hand
in that assignment, you first have to write a [*concept*]({{< ref "/assignments/deliverables/project-concept" >}}).

{{< hint info >}}
Please note, that as soon as you have handed in a first version of the [*concept*]({{< ref "/assignments/deliverables/project-concept" >}}),
you are obliged to receive a grade for this course, or in other words: your attempt to pass the course has started
and grading is inevitable.
{{< /hint >}}

Should you not pass the [*Project*]({{< ref "/assignments/project-work" >}}) assignment, you are free to take a
*[retry exam]({{< ref "/assignments/retry-exam" >}})* in order to still pass the course.


## Indication of source

{{< hint danger >}}
AI usage is implied by default unless it has being renounced explicitly (in writing). Missing indication of
source therefore automatically implies fraud attempt.
{{< /hint >}}

Every line of code or text that is not a result of your own thought process (your own brainchild), requires a
__complete__ trace of origin. One of two cases may apply:

#### (1) Copy & paste from a static source

*Examples:* web page, video, book

*Reference:* deep link (e.g. referring to the respective section of a web page or time code) or bibliographic citation

*Location:* in-place, preceding the respective line(s) 


#### (2) Inspired[^1] by AI-based[^2] interaction

*Examples:* GPTs, coding agents, chatbots

*Reference:* excerpts(s) of the prompt history[^3] that led to the respective line(s) 

*Location:* Git commit message

*Procedure:*
* split your work and the AI-based content into different commits
* break down prompt history and the related content into self-contained and comprehensible commits; group related changes 
* indicate AI-based content by prefixing the commit title with `ai:` and optionally mention the tool, like `ai(sonnet):`
* a commit message contains all parts emerged from the interaction with the AI (inputs, and if it's more that just the
  commit diff, also outputs) leading to the respective changes 
* provide the same indication of source to prompts that you did not write yourself 
* linking a prompt history is not permitted

[^1]: code or text was actually written by yourself but originates from utilizing said tools
[^2]: based on *machine learning* (commonly known as *artificial intelligence*, such as *large language models* etc.)
[^3]: human-readable format, uncensored

{{< hint danger >}}
Generally speaking, it must be made obvious to the reader, which line(s) - or sentences in case of concept or
documentation text - are affected by sources mentioned above, otherwise the deliverable __will be classified as
fraud attempt and reported__.
{{< /hint >}}
