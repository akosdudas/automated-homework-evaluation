# What is Automated Homework Evaluation?

At the **Budapest University of Technology and Economics (BME) Faculty of Electrical Engineering and Informatics (VIK)**, we use the techniques presented here for evaluating homework assignments in software development courses. This page documents the methods that I, the author, find useful and apply in my teaching profession.

!!! warning ""
    Automated Homework Evaluation is not a single software. Instead, **it is a concept, or recipe if you will**, that uses both cloud resources and custom-made software to achieve its goal.

## Problem statement

At BME VIK, we teach hundreds of students each semester. There are courses with more than 500 students in the first two years of education. And even after, what we call, choosing a specialization (like a minor), we often have 150-200 students in one class. Handling student bodies of this size require considerable resources. Thus, what we seek is **scalable education**.

Software engineering is about **solving problems**. There are lots of criteria for software, especially for good software. But first and foremost a software must **work correctly**. As educators of software engineers, we must teach students to solve the problems they will face. What better way is for them to write software that works? But then again, testing software for correct behavior is strenuous and time-consuming.

Thus, enter modern technologies. **Virtualization technologies and continuous integration systems** have shown us that running tests and setting up environments to run them can be automated. Why not use these to test not only large-scale enterprise software but comparatively small software homework assignments?

The idea, of course, is not new, and I do not claim to have figured this out first.

## The goals

The goal I set is to target automated and semi-automated homework evaluation where students write software code. The primary goal is to assess the student's practical skills. These circumstances do not cover all kinds of performance evaluations in university education. These assessments complement others, such as writing midterm tests, filling out quizzes in Moodle, submitting papers on chosen theoretical topics, etc.

**While automating evaluation, the goal is not 100% automation. Student work must be verified manually too. Automation reduces the time spent on laborious and strenuous tasks, but it does not entirely replace manual work - nor should it.**

So what are the goals?

- [x] Collect assignments easily with as little work for teachers as possible.
- [x] Review the relevant parts of the submitted code with ease. See what the student has added or changed quickly.
- [x] Automated early fail if minimum requirements are not met; e.g., code must compile, otherwise fail automatically without manual work.
- [x] Identify common problems and report these automatically to the students.
- [x] Provide feedback to the students as soon as possible. Do not wait for the submission deadline.
- [x] Allow students to improve their results based on automated feedback. Teach the students that iteration is an essential part of software development. Code is not written once but polished and "bug fixed" as long as there are issues in it.
- [x] Discuss the code with the students "inside" the source code. Show students how modern software development is carried out in teams, with code review and automation.

!!! question "Are these the right goals?"
    These goals might not be universal. These goals apply to the way I, at BME VIK, teach specific software engineering classes. You can decide whether it applies to your use cases.
