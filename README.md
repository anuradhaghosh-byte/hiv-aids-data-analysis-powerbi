# hiv-aids-data-analysis-powerbi
Power BI dashboard analysing global HIV trends (2000-2018), deaths and ART treatment coverage across 170 countries.
# Global HIV/AIDS Epidemic Dashboard: Trends, Treatment and Gaps

A two-page Power BI dashboard that looks at how big the HIV epidemic is, how it has changed since 2000, and how much of the treatment actually reaches the people who need it.

I'm a biosciences graduate moving into data analytics, and this is the project where I stopped following tutorials and tried to answer real questions with messy public data. It took longer than I expected, mostly because the data kept finding new ways to mislead me. I've written those lessons down below, because honestly they were the most useful part.

<img width="1546" height="862" alt="Screenshot 2026-10-15 225316" src="https://github.com/user-attachments/assets/4c611b4f-c562-47b4-988e-249c1b28ced2" />
<img width="1556" height="862" alt="Screenshot 2026-10-15 225624" src="https://github.com/user-attachments/assets/84f4033f-e051-4a9f-a79b-9eb139b83675" />

---

## What I wanted to find out

1. How many people are living with HIV, and is the number going up or down?
2. Are fewer people dying from it than before?
3. Which regions carry most of the burden?
4. Of the people who need treatment (ART, antiretroviral therapy), how many are actually getting it?
5. Are children and pregnant women being reached as well as everyone else?

## What the data showed

- **31.4 million** people were living with HIV in 2018 (up from 20.7 million in 2000), and about **81%** of them were in the WHO African Region.
- Deaths fell from roughly **1.22 million in 2000 to about 606,000 in 2018**. Divided by the number of people living with HIV, that is a drop from **5.9% to 1.9%**. The epidemic got bigger, but it got much less deadly.
- Treatment coverage is uneven. Across the countries that report it, the simple average ART coverage is about **56%**. Weighted by how many people actually need treatment, it is closer to **63%**. The gap between the two numbers is a reminder that small countries with high coverage can flatter an average.
- Children trail behind. Average coverage for children is about **49%**, compared with about **68%** for pregnant women receiving antiretrovirals to prevent mother-to-child transmission.
- Botswana is a good example of a hidden gap: 83% coverage overall, but only 38% for children.

## The dashboard

**Page 1: Overview.** Total people living with HIV, total deaths, death rate, a trend line for 2000 to 2018, the death rate over time, and the split by WHO region. A year selector changes every card.

**Page 2: Treatment (ART).** Average coverage for all ages, children, and pregnant women. A country table comparing adults, children and mothers receiving treatment, and a map of adult HIV prevalence (ages 15 to 49).

## The data

Six CSV files from a Kaggle HIV/AIDS dataset, all at country level (170 countries) with a WHO region for each:

| File | What it holds |
|---|---|
| People living with HIV | Estimated count, by country and year (2000, 2005, 2010, 2018) |
| Deaths | HIV-related deaths (2000, 2010, 2018; there is no 2005) |
| Adults 15 to 49 | HIV prevalence as a percentage, not a count |
| ART coverage | People receiving treatment and coverage %, a single 2018 snapshot |
| Pediatric ART coverage | Same, for children |
| Mother-to-child prevention | Pregnant women needing and receiving antiretrovirals |

Almost every estimate comes with a lower and upper bound, for example `7200 [4100-11000]`.

## How I cleaned it

- Split the `value [min-max]` text into three numeric columns (median, min, max) and used the median for analysis.
- Replaced `Nodata`, `No data` and `na` with real blanks (nulls). My first attempt turned them into zeros, which dragged average coverage down from 55.6% to 44.4%.
- Trimmed stray spaces in the Year column and thousands separators written as spaces (`4 800`), then fixed the data types. Order matters here: clean the text first, change the type last, or rows turn into errors.
- Built a country table and a year table so the six files could be filtered together.

Tools: Excel and Power Query for cleaning, Power BI and DAX for the model and visuals.

## Mistakes I made (and fixed)

These are the part I'd want a recruiter to read.

- **I added up years.** "People living with HIV" is a snapshot each year, so adding 2000, 2005, 2010 and 2018 gave 103 million. The real 2018 figure is 31 million. Fix: a single-select year slicer and measures that read the selected year.
- **I counted the same people twice.** Adults, children and pregnant women are not separate groups. Children and mothers sit inside the "all ages" number. Stacking all three double-counted them. Fix: adults are now all ages minus children, and mothers get their own chart.
- **I turned two percentages into one 100% bar.** A chart showed Namibia at 54% when its real coverage is 92%, because it was comparing one rate against another instead of showing both. Fix: show the rates side by side on a 0 to 100 axis.
- **I summed a percentage.** Percentages get averaged, or better, recalculated from the underlying counts.

## Limitations

- The numbers are estimates, and the dataset rounds them.
- Countries with no estimate are left out rather than guessed, so the 2018 total (31.4M) is lower than commonly quoted global figures.
- "Adults" here means ages 15 and over (all ages minus children). For 44 countries the dataset has no child figure, so adult numbers there are really all ages.
- The treatment tables are a 2018 snapshot only, so I can't show treatment trends over time.
- There is no male/female breakdown in these files, so I didn't build one.

## What I'd do next

- Add population data to turn counts into rates per 100,000 people.
- Look at how coverage relates to the death rate across countries.
- Find a source with sex and age breakdowns.

## Run it yourself

1. Download the six CSV files from the Kaggle dataset.
2. Open the  file in Power BI Desktop and point the data source to your local folder.
3. Select **2018** in the year slicer.

## About me

**Anuradha Ghosh**: B.Sc. in Biosciences, aspiring Data Analyst, learning SQL, Excel and Power BI.

- LinkedIn: www.linkedin.com/in/anuradha-ghosh-analyst
- Email: anuradhaghosh926@gmail.com

If you spot something I got wrong, please open an issue. I'd genuinely like to know.
