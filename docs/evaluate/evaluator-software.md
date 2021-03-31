# Evaluator software

To give you some ideas on how to write the evaluator application, let me walk you through a simplified example.

!!! note ""
    The details presented here are particular to the .NET Core platform. However, this is merely an example. Other technologies are just as applicable to automated assessment. Among the [examples](../examples.md#real-life-examples) there are evaluator applications written in Python and PowerShell.

## The sample task

The task the students have to complete is to write an ASP.NET Core WebApi application that provides a REST Api with two operations:

- `GET /api/sample/<identifier>`
    - Returns 400 Bad Request if the identifier is missing,
    - Locates the item and returns its content with 200 OK or 404 NotFound.
- `DELETE /api/sample/<identifier>`:
    - Deletes the item and returns 200 OK.

!!! note "Detailed specification"
    Note, how the [specification](../prerequisites.md#detailed-specification) is explicit about the URLs and the expected return values. This is a must in order to be able to verify them. The two "exercises" are also independent of each other, so that they can be assessed one after the other.

## Starter code

The [starter code](../prerequisites.md#starter-code) is provided to the student in this repository: <https://github.com/akosdudas/ahk-sample-startercode>. The repository contains:

- A Visual Studio [solution file](https://github.com/akosdudas/ahk-sample-startercode/blob/master/homework-sample.sln) and [project definition](https://github.com/akosdudas/ahk-sample-startercode/blob/master/homework-sample/homework-sample.csproj) with configured .NET Core version and pre-defined packages.
- `.gitignore` file so that only source code is uploaded to the repository and binaries are not.
- `neptun.txt`: [see here](../using-github/collecting-submissions.md#not-using-a-student-roster).
- A placeholder image file where the student is expected to upload a screenshot of the running application.

The project skeleton contains the following setup:

- The [entry point](https://github.com/akosdudas/ahk-sample-startercode/blob/master/homework-sample/Program.cs) of the web application according to the .NET Core WebApi platform.
- A [test controller](https://github.com/akosdudas/ahk-sample-startercode/blob/master/homework-sample/Controllers/PingController.cs) that the evaluator application can use to determine if the web application has started correctly.
- A [skeleton controller](https://github.com/akosdudas/ahk-sample-startercode/blob/master/homework-sample/Controllers/SampleController.cs) for the student to put their code.

## Evaluator application

The evaluator application is a .NET Core web application itself. Using the same technology enables hosting the student's web application _within_ the evaluator application. This is a .NET Core specific solution. An alternative would be to start the student's code and connect to it using HTTP as it is a web application.

??? question "Why hosting the student's code within?"
    There is an application written by the student that serves HTTP requests. It has a built-in web server. So, technically, we would only need to start the application and issue HTTP queries from our independent evaluator application for testing the HTTP-based queries. But in this case, we would not be able to see "inside" the application. Suppose you require the student to insert data received via HTTP request into a database. If the HTTP response to the insertion indicates success, how do you know whether the data is there in the database?

    There are two options. The first is to check the underlying database directly. The second one is to integrate with the .NET Core application written by the student and "catch" the requests from the C# code towards the database. Entity Framework Core, the de-facto database access technology for .NET Core, enables this by allowing us to subscribe to events, such as database command being executer or opening a database transaction. Hence the need to host the student's code within the tester application to subscribe to such events.

The process of evaluation is as follows.

1. The [entry point](https://github.com/akosdudas/ahk-sample-evaluator/blob/master/src/evaluator/Program.cs) of the application sets up "tasks" to execute. These are the assessments of "separate" exercises within the same assignment. There is also a pre-processing step defined here.

1. The [pre-processing step](https://github.com/akosdudas/ahk-sample-evaluator/blob/master/src/evaluator/WebAppInit.cs) sets up the environment. It starts the web application based on the student's code and checks if the connection can be established.

1. Each exercise is evaluated separately. e.g., the exercise for handling `GET` queries is assessed [here](https://github.com/akosdudas/ahk-sample-evaluator/blob/master/src/evaluator/EvaluatorEx1.cs).

1. The results are gathered [in memory](https://github.com/akosdudas/ahk-sample-evaluator/blob/master/src/ahk.common/AhkResult.cs) and are written to a text file at the end. This will make sense for the [GitHub Action-based feedback](github-actions.md).

The evaluator application is containerized, and the container will be the "distribution platform." The container is built using a [GitHub Action workflow](https://github.com/akosdudas/ahk-sample-evaluator/blob/master/.github/workflows/ahk-build-images.yml) according to the [definition file](https://github.com/akosdudas/ahk-sample-evaluator/blob/master/src/evaluator/Dockerfile) and uploaded to [GitHub Container Registry](https://github.com/users/akosdudas/packages/container/package/ahk-sample).

!!! warning "Public code and container"
    Both the evaluator application and the container are public for demonstration purposes only. I recommend not publishing these to the students in real-life.

## Alternative approach: unit tests

The evaluator application in this example is independent of the students' code. An alternative option is to include the testing code into the starter code as unit tests.

The downsides of using unit tests are the following:

- The students see explicitly what is being tested. When the evaluator application is independent and is not distributed to the students, they cannot "optimize" the solutions to fulfill the tests specifically.
- If there is an error in the evaluation logic, it is complicated to fix it. Including the evaluation login into the starter code means that the students will have _copies_ of this logic, so making a change will not be possible afterward - at least, it will not be easy. The container-based distribution allows updating the evaluation logic by pushing a new version of the container to the registry.

On the other hand, the advantages of the unit-test based approach are:

- Students being able to execute the tests locally.
- The automation of the evaluation is more straightforward than having to manage containers too.
