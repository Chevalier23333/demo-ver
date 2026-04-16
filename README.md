# mini-project-2-demo

**Author:** Mingyun Wang(https://github.com/Chevalier23333)

The original project is part of the [LSE DS105W Winter Term 2025/2026](https://lse-dsi.github.io/DS105/2025-2026/winter-term/course-info.qmd) course and aims to answer the question: **"How does travelling between Inner London and Outer London change when it is done at peak versus off-peak hours?"** with data from the [TfL Journey Planner API](https://api.tfl.gov.uk) and the [ONS Postcode Directory](https://data.london.gov.uk/dataset/postcode-directory-for-london-exp5p/). The original project is exclusively available for personnels within the LSE DS105 Course. Note that this excerpt only contains the `data collection` and `data transformation` procedures of the original project. Further analyses are not available for this demo version.

- the Inner London area is **LSE Main Campus** (as a default set by the mini project requirement, and controlling this as a single destination helps derive simplicity and standardisation over a range of omitted variables), and the Outer London boroughs: **Ealing, Sutton, Barking, and Enfield**. Reasons why I chose these are briefed in NB01.
- Some **sample postcodes** will be selected for API requests regarding journey plans based on their LSOA directories and usertypes. (*See NB01 for how those postcodes are chosen*)

- there are a few types of the investigated variables: *(note this will also appear in NB02)*
  1. `Identity Information:`
    - **postcode** (of specific spots of LSOAs subject to boroughs)
    - **timeband** (peak or offpeak)
    - **borough** (one of Ealing, Sutton, Barking, or Enfield)
    - **usertype** (binary, 0 for residential, 1 for business)
    - **IMD** (index, how deprive an LSOA is)
  2. `Time-Related Information:`
    - **duration** (averaged for inward and outword journeys)
    - **walking minutes** (averaged)
    - **transport minutes** (averaged, non-walking time during a journey, or AKA time spent on transportations)
  3. `Mode-Related Information:`
    - **complexity** (averaged, the number of transfers/transits/switches of travelling mode, obtainable by counting the number of a journey's legs)
    - **major mode** (the non-walking rode that one is estimated to spend the most time on during a journey)
  4. `Cost Information:`
    - **fare total cost** (averaged)
  - `NOTE` that several variables, as marked, are the **average** of a sample journey from outer London to inner London and the average of a sample journey form inner London to outer London. This is to balance the subtle difference between travelling inwards and outwards due to a range of reasons, like the convenience of getting to bus stops and underground stations.

- **Peak and Off-Peak Hours**: (*for more detailed definition and reason, see NB01*)
  - for **Peak Hours**, data are collected for **either 2026 Mar 19 (Thu) 08:00 or 2026 Mar 19 (Thu) 17:00**
  - for **Off-Peak Hours**, data are collected for **only 2026 Mar 19 (Thu) 12:00**

- There is a **Trial** Workflow and an **Extension** Workflow for NB01 and NB02:
  - This arrangement is because I would like to write my Data Collection and Transformation workflows with Ealing into functions, so that the amount of work will be reduced for more boroughs in terms of these repetitive jobs.
  - The **Trial** workflow picks one outer london area and one inner london area only, specifically the inner London spot will be **LSE Main Campus** (as the project's default) and the outer London borough will be **Ealing** (not a single spot, including the districts of Acton, Ealing, Greenford, Hanwell, Northolt, Perivale and Southall)
  - These choices pretty make sense because both places are Bustling Busy Business areas surrounded by/involving a wide range of businesses and residential spots. Then it would be quite reasonable to expect that we could see more intense commuting flows from A to B and a clearer difference between peak/off-peak hours.
  - The **Extension** workflow is expected to engage the rest of the boroughs: Sutton, Barking, and Enfield.
  - I complete a whole cycle of the Trial, and only then will I work with the Extension
- NB03 pools data together for analyses.

My answer to the question is: There seem to be no significant difference in terms of duration and complexity. But fares are indeed lower during off-peak hours, and there are subtle patterns related to the usertype (business or resident) and the level of deprivation of an area.

1. For postcodes in Ealing, Sutton, Barking, and Enfield, travelling between inner and outer London shows no significant difference on average in terms of journey duration and complexity during peak and off-peak hours. However, travelling during off-peak hours still requires lower fares in total for all boroughs on average.

2. During both peak and offpeak hours, travelling between inner and outer London seem especially complex for Sutton. Notably, Sutton's major mode of commuting between inner and outer London significantly differs from the mode of commuting for other boroughs: Sutton relies on bus and the national rail more than the tube system.

3. Journeys planned for Business Areas seem to be more advantaged than Journeys planned for Residential Areas when travelling between outer and inner London: Business Area journeys are shorter, cheaper, and less complex on average, and the levels of dispersion in duration, cost, and complexity also seem lower for Business areas than for Residential Areas. Such advantages exist during both peak and offpeak hours.

4. Quite counterintuitively, from the regression analyses of various variables against IMD figures for LSOAs, during both peak and off-peak hours, travelling between inner and outer london requires longer and more complex journeys for less deprived areas. Also, journey planned for less deprived areas tend to be associated with more walking minutes during each journey during both peak and offpeak hours.

5. The "Time Allowance" (see definition later) tends to decrease for less deprived areas when planning journeys between inner and outer london. This trend is true for both peak-hour and off-peak journeys.. 

## How to Run

1. Register for a TfL API key at [api-portal.tfl.gov.uk](https://api-portal.tfl.gov.uk/)

2. Clone this repository to your local machine

    ```bash
    git@github.com:Chevalier23333/demo-ver.git
    ```

3. Create a `.env` file inside `notebooks/` and add your TfL API key:

    ```text
    API_KEY=your_key_here
    (I won't show you my API key)
    ```

4. Download the [ONS Postcode Directory](https://data.london.gov.uk/dataset/postcode-directory-for-london-exp5p/) and save it to the `data/raw/` folder.

5. As you've cloned the demonstration project repository to your local machine, ensure that you have set up the file structure as below to run the project:
   mini-project-2-demo
   |_README.md
   |
   |_data
   |  |_processed
   |  |_raw
   |    |_london-postcodes-ons-postcodes-directory-feb22.csv (the file you just downloaded in step 4)
   |
   |_notebooks
     |_NB01-Data-Collection.ipynb
     |_NB02-Data-Transformation

6. Run `notebooks/NB01` to collect the data. You must consider carefully the input dates to future dates before running the notebook!!! Instructions within NB01 will tell you more about this.

    This will populate within the `data/raw/` folder with the raw JSON data.

7. Run `notebooks/NB02` to recreate processed data within the `data/processed/` folder. No specific change needed.

8. Notebook 3 involves further analyses and visualisations, but is not available for personnels out of the LSE DS Course Project.