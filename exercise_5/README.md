\# Exercise 5 — London Tube Analysis (TfL API + Station Data)



\## Objective

The goal of this exercise is to build and save several datasets about London Underground lines, stations, and passenger information using the Transport for London (TfL) Unified API and a multi-year Excel dataset.



---



\## Step-by-Step Workflow (max 10 steps)



1\. \*\*Create the folder structure\*\*  

&nbsp;  - Inside the repository, create `exercise\_5/` and a subfolder `data/` to store the resulting CSV files.



2\. \*\*Task 1 – Retrieve Tube lines from TfL API\*\*  

&nbsp;  - Use the endpoint `https://api.tfl.gov.uk/Line/Mode/tube` to get all London Underground lines.  

&nbsp;  - Extract `name`, `id`, and `lineStatuses\[0].statusSeverityDescription`.



3\. \*\*Convert to pandas DataFrame\*\*  

&nbsp;  - Build a DataFrame where each row represents one line and the columns are `name`, `id`, and `status`.



4\. \*\*Cache the results\*\*  

&nbsp;  - Save this DataFrame locally as `data/lines.csv` to avoid re-calling the API every time.



5\. \*\*Task 2 – Get stations for each line\*\*  

&nbsp;  - For each line ID, call `https://api.tfl.gov.uk/Line/{id}/StopPoints`.  

&nbsp;  - Extract station names and IDs, sorted by direction if available.  

&nbsp;  - Combine results into a DataFrame with columns like `line\_id`, `line\_name`, `station\_id`, `station\_name`.



6\. \*\*Save stations data\*\*  

&nbsp;  - Store this as `data/line\_stations.csv` for reuse.



7\. \*\*Task 3 – Load multi-year Excel data\*\*  

&nbsp;  - Download and load the “London Underground multi-year station entry and exit figures” Excel file.  

&nbsp;  - Keep only data for 2007–2017 and clean column names as needed.



8\. \*\*Compute passenger totals and enrich\*\*  

&nbsp;  - Add a new column `entries\_exits = entries + exits`.  

&nbsp;  - Merge with the `line\_stations` dataset to count how many lines (`n\_lines\_2025`) and which ones (`which\_lines\_2025`) serve each station.



9\. \*\*Create final DataFrame\*\*  

&nbsp;  - One row per station-year, with columns:  

&nbsp;    `station`, `year`, `entries\_exits`, `n\_lines\_2025`, `which\_lines\_2025`.



10\. \*\*Save and verify results\*\*  

&nbsp;   - Save as `data/station\_panel\_2007\_2017.csv`.  

&nbsp;   - Print summary info to confirm correct shape and data types.



---



\## Potential Challenges and Solutions



| Challenge | Solution |

|------------|-----------|

| \*\*API rate limits\*\* | Cache intermediate results as CSV to avoid repeated API calls. |

| \*\*Missing or inconsistent fields in API\*\* | Use `.get()` safely and default to “Unknown” for missing data. |

| \*\*Station name mismatches between API and Excel dataset\*\* | Normalize station names (strip spaces, lowercase) and merge carefully. |

| \*\*Large Excel file size\*\* | Use `usecols` and `dtype` options in `read\_excel()` to speed up loading. |

| \*\*Duplicate station records\*\* | Drop duplicates or group by `station` before merging. |



---



\## Output Files



\- `data/lines.csv` — All Tube lines and their status  

\- `data/line\_stations.csv` — Stations served by each line  

\- `data/station\_panel\_2007\_2017.csv` — Final enriched station-year dataset



