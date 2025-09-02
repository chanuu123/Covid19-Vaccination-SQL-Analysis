# Covid-19 Pandemic: A Comprehensive Data Analysis with SQL & Tableau
By Michael Olaniyi Jeremiah

![Covid19-](https://github.com/mikeolaniyi/Covid19_Vaccination_Analysis_in_SQL/assets/120651356/ab54c8f4-7465-49b9-8502-c9f758497889)


- **Brief Introduction**

 The COVID-19 pandemic reshaped lives across the globe. I embarked on a data-driven journey to uncover insights into its global impact. 



- **The Dataset:**

 Using WHO data, I analyzed the COVID-19 pandemic by exploring cases, deaths, infection trends, vaccination rollouts, and how the virus affected different continents.

 **Key Questions Explored in This Analysis:**

* What percentage of the global population has been vaccinated?
* How do COVID-19 death counts vary across continents?
* Which continent recorded the highest death rate relative to population?
* Which countries had the highest infection rates compared to population?
* What are the total reported cases and the percentage of the population infected worldwide?
* How do total cases compare to total deaths in Nigeria?


**Datasets Description:**
Covid-19 Deaths table 'CovidDeaths'  has 26 columns and 81,060 rows

![image](https://github.com/user-attachments/assets/9a3595c1-af27-4e64-a31c-1ec771ec2521).


**CovidDeaths table head output:**

![image](https://github.com/user-attachments/assets/41a25d43-6a74-40c7-aa8e-907deeafbb79)


**Covid-19 Vaccinations table 'CovidVaccinations' has 37 columns and 85,171 rows:**

![image](https://github.com/user-attachments/assets/f7455feb-625d-4f0b-8a18-3b609017164d).



![image](https://github.com/user-attachments/assets/33542f11-68a1-4190-a221-345d61fb2ac6)



**CovidVaccinations table head output:**

![image](https://github.com/user-attachments/assets/a9db9a6f-b466-450e-8c40-95713c42867c).


---------------------------------------------------------------------------------------------------------------------------------------

**Let's select the data that we are going to be using:**

```SQL
-- Let's select the data that we are going to be using

SELECT location, date, total_cases, new_cases, total_deaths, population
FROM CovidDeaths
WHERE continent is not null and  total_deaths is not null
ORDER BY 1,2
```
Output:

![image](https://github.com/user-attachments/assets/434a06de-b65d-4e06-b29e-d9a833d91f08)



## Total Cases vs Total Deaths in Nigeria
```SQL
-- Shows likelihood of dying if you contract covid in Nigeria

SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS DeathsPercentage
FROM CovidDeaths
WHERE location LIKE '%Nigeria' 
AND continent IS NOT NULL
ORDER BY 1,2,
```
Output:

![image](https://github.com/user-attachments/assets/5a7847ec-4433-4569-9ff7-34d5c328d962)

## Total Cases vs. Total Deaths in Nigeria
* The first reported COVID-19 death in Nigeria occurred on 23rd March 2020, when there were just 40 confirmed cases. By 30th April 2021, the country had recorded 165,110 confirmed cases and 2,063 deaths, giving    a death rate of approximately 1.25%—meaning that out of every 100 infected people, about 1.25 did not survive.

* Findings: Nigeria’s death rate (~1.25%) was lower than the global average (~2%), which may be attributed to factors such as a younger population or the presence of less severe virus strains during this period.

## Total Cases vs Population
```SQL
-- Shows what percentage of population infected with Covid

SELECT location, date, population, total_cases, 
	  (total_cases/population)*100 AS PercentPopulationInfected 
FROM CovidDeaths
ORDER BY 1,2
```
Output:

![image](https://github.com/user-attachments/assets/07f26211-bb19-4f13-afc6-b5f9276bf122)

**Total Cases vs. Population: Percentage of Population Infected**

* **Query Analysis:** This query highlights the percentage of each country’s population infected with COVID-19. For example, **Andorra reported an infection rate of ~17%**, while in **Nigeria**, as of **30th       April 2021**, only **0.08% of the population** (based on ~206 million people) had been reported as infected.

* **Findings:** Nigeria’s infection rate was **much lower than in many countries, especially across Europe**. This gap may point to **under-reporting of cases** or **limited testing capacity** during the period    analyzed.


**Countries with Highest Infection Rate compared to Population**
```SQL
-- Countries with Highest Infection Rate compared to Population

SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS PercentPopulationInfected
FROM CovidDeaths
WHERE continent is not null 
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC
```
 Output:
 
![image](https://github.com/user-attachments/assets/e5b8a723-12f1-4987-89c2-de33209dc2e4)

## Countries with Highest Infection Rate Compared to Population

* **Query Analysis:** The highest infection rates relative to population were recorded in smaller countries. Andorra, for instance, saw over **17% of its population** infected with COVID-19, highlighting the disproportionate impact on smaller, densely populated nations.

* **Findings:** Smaller European countries were among the hardest hit, experiencing significantly higher infection rates compared to larger nations like Nigeria.

## Countries with Highest Death Count per Population
```SQL
-- Countries with Highest Death Count per Population

SELECT location, MAX(cast(total_deaths as int)) AS TotalDeathsCount
FROM CovidDeaths
WHERE continent is not null 
GROUP BY location
ORDER BY TotalDeathsCount DESC
```
Output:

![image](https://github.com/user-attachments/assets/ff2e9b43-483f-43db-b259-be44dbc84a8a)


## Countries with Highest Death Count per Population

* Query Analysis: The United States recorded the highest total number of COVID-19 deaths, a reflection of both its large population and the widespread transmission of the virus.

* Findings: Despite having a relatively advanced healthcare system, the sheer scale of infections overwhelmed resources, resulting in a significantly high number of fatalities.

## Breaking it down by Continent
```SQL
-- Showing contintents with the highest death count per population

SELECT continent, MAX(cast(Total_deaths as int)) as TotalDeathCount
FROM CovidDeaths
WHERE continent IS NOT NULL 
GROUP BY continent
ORDER BY TotalDeathCount DESC
```
Output:

![image](https://github.com/user-attachments/assets/a963f350-9e5a-4115-a4b9-5f7c489e4737)

## Continents with the Highest Death Count per Population

* Query Analysis: **North America** recorded the highest death toll with **576,232** deaths, followed by **South America with 403,783** deaths—underscoring the severe impact of COVID-19 on these regions,           particularly in densely populated areas.

* Findings: The high death counts in North and South America were likely driven by delayed containment measures, strained healthcare systems, and variations in population health and preparedness.

## Global Covid-19 Numbers

```SQL
-- Percentage of deaths by total cases

SELECT SUM(CAST(new_cases AS INT)) AS Total_cases, 
SUM(CAST(new_deaths AS INT)) AS Total_Deaths, 
SUM(CAST(new_deaths AS INT))/ SUM(new_cases )*100 AS DeathsPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL 
ORDER BY 1,2
```   
Output:

![image](https://github.com/user-attachments/assets/5d971e3e-5fbf-419b-a798-c5d38328f1b9)

## Global Covid-19 Numbers: Death Percentage

* Query Analysis: The global death rate was calculated at **2.11%**, meaning that on average, **2 out of every 100 people** infected with COVID-19 died—a stark reflection of the pandemic’s worldwide impact.

* Findings: While this global figure highlights the severity of the crisis, regional differences in healthcare capacity, government interventions, and early response measures led to wide variations in death rates across continents.

## Total Population vs Vaccinations
```SQL
-- Shows Percentage of Population that has recieved at least one Covid Vaccine

WITH popvsvac (Continent, Location, Date, Population, New_vaccinations, RollingPeopleVaccinated) AS  
(
SELECT dea.continent, dea.location, dea.date,dea.population, vac.new_vaccinations, 
SUM(CONVERT(int,vac.new_vaccinations)) OVER (partition BY dea.location ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
FROM CovidDeaths dea
JOIN CovidVaccinations vac
ON dea.location = vac.location
and dea.date = vac.date
WHERE dea.continent IS NOT NULL
)
SELECT*, (RollingPeopleVaccinated/Population)*100 AS PercentRollingPeopleVaccinated
FROM popvsvac
```
Output:

![image](https://github.com/user-attachments/assets/e4c92b64-18ce-440c-930c-727a93da8573)

## Vaccination Rollout: Population vs. Vaccination Rates

* Query Analysis: Comparing total population against vaccination data reveals the share of people who had received at least one dose. In many countries, vaccination coverage steadily increased over time. Nations   that rolled out vaccines early and aggressively—such as the United States and several European countries—experienced a slower rise in new cases as vaccines curbed the virus’s spread.

* Findings: Vaccination played a critical role in reducing transmission, but many developing countries, including Nigeria, lagged in coverage. This slower rollout may have prolonged vulnerability to the virus.


## Covid-19 Global Impact Analysis Dashboard on Tableau

![Covid-19 Vaccination Analysis](https://github.com/mikeolaniyi/Covid19_Vaccination_Analysis_in_SQL/assets/120651356/6baecef0-83d1-46b3-89f6-1528fdd01c7b)


## Global Overview
- Total Cases: 150,574,977
- Total Deaths: 3,180,206
- Death Percentage: 2.11%

The COVID-19 pandemic had a devastating global reach, with over 150 million reported cases and more than 3 million deaths, resulting in a global death rate of 2.11%. The impact, however, varied widely across regions—Europe, North America, and South America recorded higher mortality rates compared to other continents.

## Percent Population Infected per Country

Infection rates varied widely across countries, with some experiencing a much heavier burden relative to their population size. The countries with the highest infection rates were:

* United States: 19.11%
* United Kingdom: 14.93%
* India: 8.33%

These figures show that in some nations, nearly 1 in 5 people contracted the virus, placing immense pressure on their healthcare systems.

## Recommendations


### Accelerating Vaccine Distribution in Developing Nations

* The data shows that countries with higher vaccination rates experienced slower infection growth and fewer deaths. For low- and middle-income nations like Nigeria, it is crucial to prioritize vaccine procurement and distribution to protect larger portions of the population. Achieving this will require international collaboration and strong support from organizations such as the WHO, ensuring equitable access to vaccines worldwide.

### Improve Testing and Reporting Infrastructure

* Infection rates in countries such as Nigeria point to possible under-reporting caused by limited testing capacity. Strengthening healthcare infrastructure—with greater investment in testing facilities, data reporting, and contact tracing systems—is essential for early detection and effective control of future pandemics.

### Strengthen Public Health Education and Communication

* Countries that achieved lower COVID-19 mortality rates often had clear public health messaging and consistent guidelines. To replicate this success, governments should invest in educating communities on vaccination, social distancing, mask usage, and other preventive measures—with a particular focus on regions where vaccine hesitancy remains high.

### Enhance Pandemic Preparedness Plans

* Countries with overwhelming death rates—such as the United States and Brazil—highlighted the consequences of limited preparedness. To avoid similar outcomes, all nations should establish comprehensive pandemic response plans, including stockpiling medical supplies, PPE, and ventilators, while also expanding ICU capacity. Regular simulation exercises and preparedness drills are essential to ensure readiness for future outbreaks.

### Strengthen Global Health Collaboration

* The findings highlight the critical importance of international cooperation in pandemic management. Nations must work together to share data, resources, and best practices, while organizations like the WHO should be further empowered to coordinate timely interventions across borders.

### Target Vulnerable Populations

* High-risk groups—such as the elderly, immunocompromised individuals, and those with pre-existing conditions—should be prioritized during pandemics. Dedicated programs must ensure these populations have equitable access to vaccines, healthcare, and social support services.

### Invest in Long-Term Healthcare Resilience

* To prepare for future outbreaks, countries need to strengthen their healthcare systems through greater investment in funding, workforce training, and scalable infrastructure. Building resilience today ensures healthcare systems can withstand the pressure of large-scale pandemics tomorrow.


By adopting these recommendations, countries can be better equipped to manage future pandemics, reduce loss of life, and minimize disruptions to global health systems. The COVID-19 crisis has underscored the urgent need for proactive, unified, and well-prepared responses to global health challenges.


