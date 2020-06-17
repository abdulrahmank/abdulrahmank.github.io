---
layout: default
title:  "Being a well balanced developer"
description: Sharing my views on how to be a well balanced developer - Part 1.
date:   2020-06-14 00:00:00 -0000
categories: main
---

# Being a well balanced developer - Part 1 #

When it comes to software developments, Developer's role is not just to code. As a developer we need to ask ourselves two questions:
1. **Are we building the right product?**

    Answering this will give us more clarity on all the non functional requirement like the usability etc, as we tend to understand our stake holders, what problems of the end users are we trying to solve etc. Without answering this question the satisfaction of having delivered a quality software cant be achieved, also our product will soon be replaced. 

    Answering this question also contributes to another important topic of software which is **scoping**. By better 
    scoping of the features of the product, we can gain early engagement with the user and also get a better sense of the user behaviour. By scoping, we can also drastically _reduce the **Time To Market (TTM)** of the product as a whole_.

2. **Are we building the product right?**

    When it comes to programming, as a good developer we need to follow principles like:
    - YAGNI
    - DRY
    - KISS
    - SOLID

    Also we need to orient the code base in any paradigm (like OOPS/FP/Reactive programming) and stick to one convention through out the project.

    Sticking to one convention may be boring for an enthusiastic developer(I have been there 😄). But later when dealing with adding features to the code which didn't follow convention or debugging the code, this will bring down the enthusiasm of the developer working on it 🙄. More frustrating will be that we will not remember why we wrote those flabergasting code, when the whole team recognises us as the person with most context and the one who developed and delivered successfully. Thus one of the main trait of a well balanced developer is to follow 
    the convention followed in the codebase. 
    
    This doesn't mean we have to follow the convention blindly, ask the right set of questions and 
    note down the pro's and con's of the conventions followed. If there is some disadvantage in following the 
    convention, try to weigh the cost of maintainance of the new convention vs the disadvantage. If the disadvantage
    of following existing code is huge (like it will make maintenance very hard, poses a security threat, etc), then follow a **Strangulation approach**, where we can start developing new features using the new pattern/
    convention and have tech debt cards for migrating the existing features to new pattern. 

    This question can be answered in a pro level by test coverage. Unit tests are always overlooked. To emphasize unit testing, and to ease following the principles like YAGNI, we can try following Test driven development (TDD) as well.

    If we build the product right, then the time taken for a new feature addition should drastically reduce, while the internal quality (Readability of code, unit test coverage etc) and external quality (like less bugs, reliability) of the product should increase.
