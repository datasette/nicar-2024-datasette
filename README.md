# NICAR24 Datasette Workshop

[Friday March 8th, 11:30am](https://schedules.ire.org/nicar-2024/index.html#1110) in [Baltimore, Maryland](https://schedules.ire.org/nicar-2024)

Your instructors:

- Simon Willison
- Alex Garcia

## Questions and shared notes

- We will be monitoring [this Google Doc](https://docs.google.com/document/d/1OyEjk7yoQMdyhkAccRvg7R17nEHpIXpSs8BBIH78wWU/edit) during the workshop for questions. Please feel free to use it to share notes as well - things that we missed can then be incorporated into the tip sheet after the session.

## Getting Help

- [Datasette on Discord](https://datasette.io/discord)
- The `#proj-datasette` channel in the [News Nerdery Slack](https://newsnerdery.org/)
- The [Github Discussions page on Datasette](https://github.com/simonw/datasette/discussions)

## How to run Datasette on a laptop

TODO Alex

## Introduction to Datasette Cloud

Tip sheet outline:

Datasette Cloud is a collaborative space for your newsroom to share and analyze data

It's all built on open source Datasette components: if you want to build your own you are welcome to do so! It will probably cost you more time than having us run it for you though.

Each organization gets a "space" - a private space to collaborate on data.

Let's create a space now:

- Go to https://www.datasette.cloud/ and create an account - I recommend sign in with Google, that way you don't have to deal with Yet Another Password.
- Enter the invite code we distributed in the session...
- ... and create a space. Datasette Cloud is built on top of Fly.io which means we can run your space in many different locations around the world. The default in Virginia works just fine though.
- Spaces can take up to a minute to be created the first time.
  - Each space runs on a separate container, for security and to ensure the performance of one space doesn't impact any others
  - We'll start you with 2GB of volume space but this can be increased up to 500GB
  - Your data is continually backed up to a private S3 bucket using Litestream. You can download snapshots of the data directly.
  - Philosophically, avoiding lockin is very important to us. You should be able to extract your data at any time, in an open format
- Once the space is up and running! Let's import some data. There are several ways to load data into Datasette Cloud:
  - Uploading CSV files
  - Importing CSV files from a URL
  - Using the Datasette Cloud API
  - ... and a new option using AI, which we'll try out shortly
- Let's start with an import from a URL - we'll use the Global Power Plants example on the site
  - Click "The Global Power Point Database" to get started
  - Once it has imported, rename that table:
    - Table actions -> Edit table schema -> Enter a new name -> Click "Rename"
  - And we get our first visualization! Because it has latitude and longitude columns we can see it on a map.
- Next we'll upload some CSV data.
  - Download a copy of the CSV of Baltimore Grocery Stores from https://data.baltimorecity.gov/datasets/baltimore::grocery-stores/explore
  - Click "Upload CSV files" on the homepage and drag on the file
  - This gets a map too!
- Let's try a larger CSV
  - Download the CSV of Baltimore City Employee Salaries from https://catalog.data.gov/dataset/baltimore-city-employee-salaries-b820d
  - Upload the file
  - Edit schema to change the type on `annualSalary` and `grossPay` to float. This means we can sort them.
  - Let's start exploring! Find the highest paid employee.
  - We can run a custom SQL query to see the department with the highest average:
    ```sql
    select agencyName, avg(grossPay) from Baltimore_City_Employee_Salaries
    group by agencyName
    order by avg(grossPay) desc
    ```
- Now we'll use an enrichment to add a `hireYear` column:
  - Table actions -> Enrich selected data -> Regular expressions
  - Source column: `hireDate`
  - Capture mode: "Store first match in single column"
  - Regular expression: `(\d{4})-.*`
  - Output column: `hireYear`

# Demo: `datasette-scribe`

TODO Alex
