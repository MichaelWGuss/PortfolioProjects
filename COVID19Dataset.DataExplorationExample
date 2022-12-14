Select * 
From PortfolioProject..CovidDeaths
order by 3,4

--Select * 
--From PortfolioProject..CovidVaccinations
--order by 3,4

-- Select data that we are going to be using

Select Location, date, total_cases, new_cases, total_deaths, population
From PortfolioProject..CovidDeaths
order by 1,2


-- Looking at Total Cases vs Total Deaths
-- Shows likelihood of dying if you contract covid in your country
Select Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
From PortfolioProject..CovidDeaths
where location like '%states%'
order by 1,2


--Looking at Total Cases vs Population
--Show what percentage of population got covid-19

Select Location, date, Population, total_cases, (total_cases/population)*100 as PercentInfected
From PortfolioProject..CovidDeaths
where location like '%states%'
AND Total_cases is not null
order by 1,2

-- Looking at countries with highest infection rate compared to population

Select Location, Population, MAX(total_cases) as HighestInfectionCount, MAX((total_cases/population))*100 as PercentInfected
From PortfolioProject..CovidDeaths
Group by Location, Population
order by PercentInfected DESC


--Showing Countries with Highest Death Count per Population
---Added Where clause due to table showing null where location shows a continent. Adding this eliminates non-country specific returns.

Select Location, MAX(cast(Total_deaths as int)) as TotalDeathCount
From PortfolioProject..CovidDeaths
Where continent is not null
Group by Location
order by TotalDeathCount DESC

--Looking at continents with highest death count per population as well

Select continent, MAX(cast(Total_deaths as int)) as TotalDeathCount
From PortfolioProject..CovidDeaths
Where continent is not null
Group by continent
Order by TotalDeathCount Desc

--looking at global numbers

Select SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(New_Cases)*100 as DeathPercentage
From PortfolioProject..CovidDeaths
where continent is not null
--group by date
order by 1,2

--Looking at total population vs vaccinations

Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(cast(vac.new_vaccinations as int)) over (partition by dea.location order by dea.location, dea.date) as RollingCountPeopleVaccinated -- can also convert(int,
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	on dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null
and dea.population is not null
order by 2,3


--Using CTE 

with PopvsVac (Continent, Location, Date, Population, new_vaccinations, RollingCountPeopleVaccinated)
as
(
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(cast(vac.new_vaccinations as int)) over (partition by dea.location order by dea.location, dea.date) as RollingCountPeopleVaccinated -- can also convert(int,
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	on dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null
and dea.population is not null
--order by 2,3
)
select *, RollingCountPeopleVaccinated/Population
From PopvsVac


--Temp table

Drop table if exists #PercentPopulationVaccinated
Create Table #PercentPopulationVaccinated
(
Continent nvarchar(255),
location nvarchar(255),
date datetime,
population numeric,
new_vaccinations numeric,
RollingCountPeopleVaccinated numeric
)

Insert into #PercentPopulationVaccinated
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(int,vac.new_vaccinations)) over (partition by dea.location order by dea.location, dea.date) as RollingCountPeopleVaccinated -- can also convert(int,
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	on dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null
and dea.population is not null
--order by 2,3

select *, RollingCountPeopleVaccinated/Population
From #PercentPopulationVaccinated

-- Creating view to store data for visualizations

Create View PercentPopulationVaccinated as
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(cast(vac.new_vaccinations as int)) over (partition by dea.location order by dea.location, dea.date) as RollingCountPeopleVaccinated -- can also convert(int,
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	on dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null
and dea.population is not null
--order by 2,3


Select * 
from PercentPopulationVaccinated

Create View TotalDeathCount as
Select location, MAX(cast(Total_deaths as int)) as TotalDeathCount
From PortfolioProject..CovidDeaths
Where continent is not null
Group by Location
order by TotalDeathCount DESC

Create View PercentUSAPopInfected as
Select Location, date, Population, total_cases, (total_cases/population)*100 as PercentInfected
From PortfolioProject..CovidDeaths
where location like '%states%'
AND Total_cases is not null
--order by 1,2

