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

This will be a walkthrough of the Datasette open source project! This can be ran either on the conference-provided laptops, as long as you have a working Python environment (ie can `pip install`) packages.

### 1. Installing Datasette and `sqlite-utils`

On the command line, run:

```
pip install datasette sqlite-utils
```

To make sure it installed correctly:

```
datasette --version

sqlite-utils --version
```

For this workshop, we will use the [`datasette-upload-csvs`](https://github.com/simonw/datasette-upload-csvs) plugin, which can be installed with:

```
datasette install datasette-upload-csvs
```

### 2. Uploading a CSV

We will be working with the [San Fransisco Supplier Contracts](https://data.sfgov.org/City-Management-and-Ethics/Supplier-Contracts/cqi5-hm2d/about_data)
dataset. [Download the CSV here](https://gist.githubusercontent.com/asg017/144d059dbad77135a50ef3cd8590aad5/raw/sf_supplier_contracts.csv).

Now startup Datasette with an empty database like so:

```
datasette --create nicar24.db --root
```

<details>
<summary>Explanation of flags</summary>
<li>The <code>--create</code> flag will create the <code>nicar24.db</code> database for you. Without it, an <code>nicar24.db file not found</code> error would be raised</li>
<li>The <code>--root</code> flag will print out a signed URL, which grants a view higher privileges, like uploading CSVs.
</details>

Copy+paste the `http://127.0.0.1:8001/-/auth-token?token=...` URL into a web browser. You should see "root" in the top-right corner.

Navigate to the top-right corner menu and select "Upload CSVs". Drag+ drop the `sf_supplier_contracts.csv` file, and name the table `sf_supplier_contracts`.

After importing is complete, you'll have a new `sf_supplier_contracts` table to explore!

### 3. Building a search engine on NICAR24 Sessions

Now we'll focus on a 2nd way to import data, using the `sqlite-utils` CLI. Download the [`nicar-2024-schedule.csv`](https://schedules.ire.org/nicar-2024/nicar-2024-schedule.csv) file to your project folder.

To import the CSV to your SQLite database, we'll use `sqlite-utils` like so:

```
sqlite-utils insert nicar24.db sessions nicar-2024-schedule.csv  --csv
```

Start Datasette back up with:

```
datasette nicar24.db
```

<details>
<summary>No need for the <code>--create</code> or <code>--root</code> flags!</summary>
Since the <code>nicar24.db</code> database has been created, and we aren't uploading CSVs through the Datasette interface anymore, there's no need for those flags anymore.
</details>

Open `http://127.0.0.1:8001` and checkout the new `sessions` table.

If we want to add a search field to the session_description column, we could run:

```
sqlite-utils enable-fts nicar24.db sessions session_description
```

## Introduction to Datasette Cloud

TODO Simon

# Demo: `datasette-comments`

(or is this a part of Datasette cloud?)

# Demo: `datasette-scribe`

TODO Alex
