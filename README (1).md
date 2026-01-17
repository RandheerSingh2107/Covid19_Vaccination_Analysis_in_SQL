
# 📊 COVID-19 Data Analysis Using SQL  

![SQL](https://img.shields.io/badge/SQL-Microsoft%20SQL%20Server-blue)
![Data Analysis](https://img.shields.io/badge/Domain-Data%20Analytics-green)
![Status](https://img.shields.io/badge/Project-Completed-success)

---

## 📝 Project Overview  

The COVID-19 pandemic was a global crisis that affected millions of lives worldwide.  
This project performs **end-to-end SQL analysis on COVID-19 deaths and vaccination data** to extract insights related to infection rates, death rates, and vaccination progress across countries and continents.

This is a **portfolio-ready Data Analyst project**, demonstrating strong SQL skills such as **joins, CTEs, window functions, temp tables, and views**.

---

## 🛠️ Tools & Technologies  

- **Database:** Microsoft SQL Server  
- **Language:** SQL  

**Concepts Used**
- Joins  
- Aggregate Functions  
- Window Functions  
- CTEs  
- Temporary Tables  
- Views  
- Data Type Casting  

---

## 📂 Dataset Information  

**Source:** World Health Organization (WHO)

### Tables Used  

**CovidDeaths**
- location  
- date  
- total_cases  
- new_cases  
- total_deaths  
- population  
- continent  

**CovidVaccinations**
- location  
- date  
- new_vaccinations  

---

## 🎯 Business Questions Answered  

- What is the global COVID-19 death percentage?  
- Which countries have the highest infection rates?  
- Which countries and continents have the highest death counts?  
- What percentage of the population has been vaccinated?  
- What is the relationship between total cases and total deaths?  
- COVID-19 impact analysis focused on Nigeria  

---

## 🔍 SQL Queries & Analysis  

### 1️⃣ View Filtered COVID Deaths Data  

```sql
SELECT *
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 3,4;
```

---

### 2️⃣ Selected Columns for Analysis  

```sql
SELECT location, date, total_cases, new_cases, total_deaths, population
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 1,2;
```

---

### 3️⃣ Total Cases vs Total Deaths (Nigeria)  

```sql
SELECT location, date, total_cases, total_deaths,
       (total_deaths / total_cases) * 100 AS DeathsPercentage
FROM PortfolioProject..CovidDeaths
WHERE location LIKE '%Nigeria'
AND continent IS NOT NULL
ORDER BY 1,2;
```

---

### 4️⃣ Total Cases vs Population  

```sql
SELECT location, date, population, total_cases,
       (total_cases / population) * 100 AS PercentPopulationInfected
FROM PortfolioProject..CovidDeaths
ORDER BY 1,2;
```

---

### 5️⃣ Countries with Highest Infection Rate  

```sql
SELECT location, population,
       MAX(total_cases) AS HighestInfectionCount,
       MAX((total_cases / population)) * 100 AS PercentPopulationInfected
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC;
```

---

### 6️⃣ Countries with Highest Death Count  

```sql
SELECT location,
       MAX(CAST(total_deaths AS INT)) AS TotalDeathsCount
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY TotalDeathsCount DESC;
```

---

### 7️⃣ Death Count by Continent  

```sql
SELECT continent,
       MAX(CAST(total_deaths AS INT)) AS TotalDeathCount
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY TotalDeathCount DESC;
```

---

### 8️⃣ Global COVID-19 Numbers  

```sql
SELECT SUM(new_cases) AS TotalCases,
       SUM(CAST(new_deaths AS INT)) AS TotalDeaths,
       SUM(CAST(new_deaths AS INT)) / SUM(new_cases) * 100 AS DeathPercentage
FROM PortfolioProject..CovidDeaths
WHERE continent IS NOT NULL;
```

---

### 9️⃣ Population vs Vaccination (Window Function)  

```sql
SELECT dea.continent, dea.location, dea.date, dea.population,
       vac.new_vaccinations,
       SUM(CONVERT(INT, vac.new_vaccinations))
       OVER (PARTITION BY dea.location ORDER BY dea.date)
       AS RollingPeopleVaccinated
FROM PortfolioProject..CovidDeaths dea
JOIN PortfolioProject..CovidVaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL;
```

---

### 🔟 Vaccination Percentage using CTE  

```sql
WITH PopVsVac AS (
    SELECT dea.continent, dea.location, dea.date, dea.population,
           vac.new_vaccinations,
           SUM(CONVERT(INT, vac.new_vaccinations))
           OVER (PARTITION BY dea.location ORDER BY dea.date)
           AS RollingPeopleVaccinated
    FROM PortfolioProject..CovidDeaths dea
    JOIN PortfolioProject..CovidVaccinations vac
    ON dea.location = vac.location
    AND dea.date = vac.date
    WHERE dea.continent IS NOT NULL
)
SELECT *,
       (RollingPeopleVaccinated / population) * 100 AS PercentVaccinated
FROM PopVsVac;
```

---

## 📊 SQL Views Created  

- PercentPopulationVaccinated  
- GlobalDeaths  
- Death_by_Continent  
- CountryWithHighestDeaths  
- HighestInfectionRate  
- CasesvsDeaths  

---

## 📈 Key Insights  

- Countries show varying infection and death rates  
- Vaccination progress differs significantly across regions  
- Window functions help analyze cumulative trends  
- SQL views enable seamless BI dashboard creation  

---

## 🚀 How to Run This Project  

1. Import datasets into **Microsoft SQL Server**
2. Execute SQL queries in sequence
3. Query views for insights
4. (Optional) Connect to **Power BI / Tableau**  

---

## 📌 Portfolio Value  

✔ Real-world dataset  
✔ Advanced SQL techniques  
✔ Interview-ready project  
✔ BI-ready output  

---

## 👤 Author  

**Randheer Singh**  
🎯 Aspiring Data Analyst  

---

⭐ If you like this project, don’t forget to star the repository!
