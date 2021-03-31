# Prerequisite for automated evaluation

The first step in creating an automated evaluation assignment is determining whether a particular homework or project is suitable. Let me give you some pointers for what might work.

## Technology

First and foremost, technology must be available to automatically and headlessly run the code submitted by the student. Most programming languages and databases, nowadays, offer this out of the box. E.g., you can easily compile C, C++, C#, Java, Ruby, Php, Go, etc., code and run them in a virtualized environment.

Some operating systems might be prohibitive, though. While Windows, Linux, and OSX environments are easy targets (for example, GitHub Actions CI can run on either platform), other environments using dedicated hardware, like FPGA or mobile phone operating systems, might not be suitable. You need to find out whether you can write a script that can execute in a CI system.

## The tasks must enable automated checking

Not every software-related assignment can be automatically assessed. Creative actions, such as designing an architecture, are, by their nature, up for interpretation. Similarly, checking the contents of an image is not feasible with reasonable efforts. Code writing assignments are a better fit, but even here, not all tasks will work out.

The expected result must deterministic and must be available for access after execution. E.g., checking the database records' contents is good, but checking the memory state is probably too complicated. And the result (e.g., the outcome of executing the software, the return value of a method, the status code of an HTTP response, etc.) must be such that a few lines of code can determine its correctness.

So how can you determine whether automated evaluation can work for you? Try to write a script or program to check the submission of a student. Imagine that you receive the submission file and then automate what you would check manually.

## Starter code

Provide a starter code to enforce file names and folder structure. If you want to compile the code or run the executable, you must know the file names. Most modern IDEs provide an umbrella, like a project, solution, workspace, package, or similar item, to describe an environment. But even if there is no IDE for the chosen platform, you can create empty files with the required names. Providing Docker-based environments (i.e., a `docker-compose.yml`) that provision the environment with a single command works too.

The starter code is also helpful to define the environment specifically. Most IDEs and projects, packages, solutions, etc., contain a description of the build and runtime environment. E.g., are you using Python 3.6 or 3.8, .NET Framework 4.8, or .NET Core 3.1? These settings should go into the starter code.

## Detailed specification

The tasks must be specified in detail and leave as little up for interpretation as possible.

Some examples:

- If you want to check that the C# class has a method that does some action, the method's name must be specified.
- If the webserver must return a specified content, the URL where this content resides must be specified.
- If the task is to store data in a database, the table name and column names must be specified.

!!! tip "Reflection"
    Some languages offer leniency here. E.g., you can use reflection in C# to find a method in a class, even if the name is not known. Such techniques offer great possibilities. Specifications in real-life scenarios rarely provide the name of every method and field of a class. Therefore, the specification can be less strict when using reflection to "find" the code parts that need checking.

!!! note "Improved in iterations"
    There is a single way the specification should be interpreted, but it will be misinterpreted in countless ways. Having hundreds of students "checking" your work will eventually help you word the specification precisely.
