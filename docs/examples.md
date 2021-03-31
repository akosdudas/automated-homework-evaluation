# Show me an example!

## The _hello world_ example

The complete picture is demonstrated here: <https://github.com/akosdudas/ahk-sample-studentsolution/pull/1>

This GitHub pull request represents a homework submission with automated evaluation. The student receives a starter code and extends it to fulfill the required tasks. The code the student added is visible [here](https://github.com/akosdudas/ahk-sample-studentsolution/pull/1/files). (This is, of course, a dummy example.) The automated evaluation reports the assessment in comments [here](https://github.com/akosdudas/ahk-sample-studentsolution/pull/1#issuecomment-646669774) and [here](https://github.com/akosdudas/ahk-sample-studentsolution/pull/1#issuecomment-646672461).

!!! success "What do you see in this example?"
    - Submissions are "handed in" at a centralized location (GitHub). No emails, no zip files.
    - The final changes of the source code are highlighted. The teacher can comment on the source code directly, and it will be visible to the student.
    - GitHub Actions CI runs automated checks and fails the submission if minimum requirements are not met.
    - Custom-made software evaluates the solution, assigns points, and reports errors.
    - Students can re-submit an improved solution and get instant feedback again.

## Real-life examples

Here are a couple of real-life scenarios in which my colleagues at BME VIK and I used the concept.

- Microsoft SQL Server server-side programming (stored procedures, triggers): students have to write SQL scripts which are then evaluated for correctness in MSSQL server (e.g., does the stored procedure perform the required action).
- ASP.NET Core WebApi application written in C# using Entity Framework for database access: students write a REST API which is tested automatically; the expected end-result is validated in the REST responses and in the database too.
- Microsoft SQL Server Integration Services ETL process: CSV input files are processed by executing the ETL process the student creates. The assessment checks the result in the target database.
- Elasticsearch queries and Kibana visualizations: students write queries in Elasticsearch (JSON files) and create visualizations in Kibana (exported as JSON). The files are verified for key content.
- C# class inheritance: students create a class inheritance structure to showcase properly using object-oriented concepts. The code is verified with Roslyn to check, for example, whether specific classes have the necessary interfaces.
- Windows Forms UI: students create a user interface, which is verified with Visual Studio Coded UI Tests, e.g., whether a button exists on Form, etc.
