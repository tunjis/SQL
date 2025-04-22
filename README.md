# 📊  SQL Portfolio Showcase: World Database Analysis <a href="https://www.mysql.com/" target="_blank" rel="noreferrer"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="36" height="36" alt="MySQL"/> **MySQL** </a>&nbsp;

<a href="https://www.mysql.com/" target="_blank" rel="noreferrer"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="36" height="36" alt="MySQL"/> </a>&nbsp; [word db(1).sql](https://justit831-my.sharepoint.com/:u:/g/personal/justincracium_bootcamp_justit_co_uk/EYFUDcPxgehEhuULTiBi8-EBkN6T5jyUOKS7JTGi18Ei1w?e=xqeYqF)


**Objective:** To demonstrate proficiency in SQL by querying the `world` database to answer a variety of analytical questions, showcasing skills in data retrieval, filtering, aggregation, and joining techniques.

**Key Activities & Skills Demonstrated:**

* **Database Setup:** Utilised the standard `world` database schema containing information about countries, cities, and languages.
* **Data Retrieval (`SELECT`):** Extracted specific columns and all columns (`*`) from tables. Used `AS` to create meaningful aliases for columns.
* **Filtering (`WHERE`):** Applied various conditions using operators like `=`, `>`, `BETWEEN`, `LIKE` (with `%` wildcard), and `IS NOT NULL` to isolate specific data subsets.
* **Sorting (`ORDER BY`):** Arranged results in ascending (`ASC`) and descending (`DESC`) order based on column values.
* **Limiting Results (`LIMIT`):** Restricted the number of rows returned, including using offsets to fetch specific ranges.
* **Aggregation (`GROUP BY`, Aggregate Functions):** Grouped data using `GROUP BY` and performed calculations using functions like `COUNT()` and `AVG()`. Applied `ROUND()` for cleaner numerical output.
* **Joining Tables (`JOIN`):** Combined data from multiple tables (`city`, `country`) based on related columns (e.g., `CountryCode`, `Capital ID`) using `JOIN` clauses (implicitly INNER JOIN).
* **Calculations:** Performed arithmetic calculations within queries (e.g., Population Density, GNP per Capita).
* **Subqueries:** Used subqueries for complex filtering conditions (e.g., finding cities in countries with above-average GNP per capita).

---

## 💡 SQL Query Examples & Scenarios

Here are the specific SQL queries used to address the analytical tasks based on the `world` database:

1.  **Count Cities in USA:** Determine the total number of cities within the United States.
    ```sql
    SELECT COUNT(*) AS TotalUSACities
    FROM city
    WHERE CountryCode = 'USA';
    ```

2.  **Country with Highest Life Expectancy:** Identify the country with the highest life expectancy.
    ```sql
    SELECT Name, LifeExpectancy
    FROM country
    WHERE LifeExpectancy IS NOT NULL
    ORDER BY LifeExpectancy DESC
    LIMIT 1;
    ```

3.  **Cities Containing 'New':** Compile a list of world cities whose names include 'New'.
    ```sql
    SELECT Name, CountryCode
    FROM city
    WHERE Name LIKE '%New%'
    ORDER BY Name;
    ```

4.  **Top 10 Most Populous Cities:** List the first 10 cities by population globally.
    ```sql
    SELECT Name, CountryCode, Population
    FROM city
    ORDER BY Population DESC
    LIMIT 10;
    ```

5.  **Cities with Population > 2 Million:** Identify cities with populations exceeding 2 million.
    ```sql
    SELECT Name, CountryCode, Population
    FROM city
    WHERE Population > 2000000
    ORDER BY Population DESC;
    ```

6.  **Cities Starting with 'Be':** Compile a list of cities starting with the prefix 'Be'.
    ```sql
    SELECT Name, CountryCode
    FROM city
    WHERE Name LIKE 'Be%'
    ORDER BY Name;
    ```

7.  **Mid-Sized Cities (Population 500k-1M):** Identify cities with populations between 500,000 and 1 million.
    ```sql
    SELECT Name, CountryCode, Population
    FROM city
    WHERE Population BETWEEN 500000 AND 1000000
    ORDER BY Population DESC;
    ```

8.  **Cities Sorted Alphabetically:** Provide a list of all cities sorted by name (A-Z).
    ```sql
    SELECT Name, CountryCode
    FROM city
    ORDER BY Name ASC;
    ```

9.  **Most Populated City:** Identify the single most populated city in the database.
    ```sql
    SELECT Name, CountryCode, Population
    FROM city
    ORDER BY Population DESC
    LIMIT 1;
    ```

10. **City Name Frequency Analysis:** List unique city names alphabetically with their counts.
    ```sql
    SELECT Name, COUNT(*) AS Frequency
    FROM city
    GROUP BY Name
    ORDER BY Name ASC;
    ```

11. **City with Lowest Population:** Identify the city with the smallest population.
    ```sql
    SELECT Name, CountryCode, Population
    FROM city
    ORDER BY Population ASC
    LIMIT 1;
    ```

12. **Country with Largest Population:** Identify the country with the highest population.
    ```sql
    SELECT Name, Population
    FROM country
    ORDER BY Population DESC
    LIMIT 1;
    ```

13. **Capital of Spain:** Identify the capital city of Spain.
    ```sql
    SELECT city.Name AS CapitalCity
    FROM country
    JOIN city ON country.Capital = city.ID
    WHERE country.Code = 'ESP';
    ```

14. **Cities in Europe:** Compile a list of cities located on the continent of Europe.
    ```sql
    SELECT city.Name AS CityName, country.Name AS CountryName
    FROM city
    JOIN country ON city.CountryCode = country.Code
    WHERE country.Continent = 'Europe'
    ORDER BY city.Name ASC;
    ```

15. **Average City Population by Country:** Calculate the average city population for each country.
    ```sql
    SELECT
        country.Name AS CountryName,
        ROUND(AVG(city.Population), 0) AS AverageCityPopulation
    FROM
        city
    JOIN
        country ON city.CountryCode = country.Code
    GROUP BY
        country.Code, country.Name
    ORDER BY
        country.Name ASC;
    ```

16. **Capital Cities Population Comparison:** List capital cities worldwide, ordered by population.
    ```sql
    SELECT
        city.Name AS CapitalCity,
        country.Name AS CountryName,
        city.Population AS CapitalPopulation
    FROM
        city
    JOIN
        country ON city.ID = country.Capital
    ORDER BY
        city.Population DESC;
    ```

17. **Countries with Low Population Density:** Identify the 25 countries with the lowest population density (people per square km).
    ```sql
    SELECT
        Name,
        Population,
        SurfaceArea,
        ROUND((Population / SurfaceArea), 2) AS PopulationDensity_PerSqKm
    FROM
        country
    WHERE
        SurfaceArea > 0 AND Population > 0
    ORDER BY
        PopulationDensity_PerSqKm ASC
    LIMIT 25;
    ```

18. **Cities with High GNP per Capita (Country Level):** Identify cities located in countries with an above-average Gross National Product (GNP) per capita.
    ```sql
    SELECT
        city.Name AS CityName,
        country.Name AS CountryName,
        ROUND((country.GNP / country.Population), 2) AS CountryGNP_PerCapita
    FROM
        city
    JOIN
        country ON city.CountryCode = country.Code
    WHERE
        country.Population > 0 AND country.GNP > 0
        AND (country.GNP / country.Population) > (
            SELECT AVG(GNP / Population)
            FROM country
            WHERE Population > 0 AND GNP > 0 AND GNP IS NOT NULL
        )
    ORDER BY
        CountryGNP_PerCapita DESC,
        country.Name ASC,
        city.Name ASC;
    ```

19. **Cities Ranked 31-40 by Population:** List cities ranked 31st to 40th by population.
    ```sql
    SELECT Name, CountryCode, Population
    FROM city
    ORDER BY Population DESC
    LIMIT 30, 10; -- Skip 30 rows, take the next 10
    ```

---

### Summary of Key SQL Skills Demonstrated:

* **Data Querying:** `SELECT`, `FROM`, `WHERE`.
* **Filtering:** Comparison operators (`=`, `>`, `BETWEEN`), `LIKE`, `IS NOT NULL`.
* **Sorting & Limiting:** `ORDER BY` (`ASC`, `DESC`), `LIMIT` (including offset).
* **Aggregation:** `GROUP BY`, `COUNT()`, `AVG()`.
* **Joining:** `JOIN ... ON` (primarily INNER joins demonstrated).
* **Data Manipulation:** Aliasing (`AS`), basic arithmetic, `ROUND()`.
* **Subqueries:** Used for filtering based on aggregated results.

---

### 🧑‍💻 Created by [tunjis](https://github.com/tunjis) 

 

* 🌍  Based in <a href="https://maps.app.goo.gl/hMxhRX5ptQAAkL7NA/" target="_blank">**London**</a>
* 🖥️  See my portfolio at [Data’s the new oil. I’m the refinery.](https://github.com/tunjis?tab=repositories)
* 📫  Contact me via my [LinkedIn profile](https://linkedin.com/in/justincraciun/)
* 🧠  Learning Data Science
* 🤝  Open to collaborating on interesting projects
* ⚡  AI enthusiast

---

### 🛠️ Technical Skills
<a href="https://www.python.org/" target="_blank" rel="noreferrer"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="36" height="36" alt="Python"/> **Python** </a>&nbsp;
<a href="https://www.microsoft.com/en-us/microsoft-365/excel" target="_blank" rel="noreferrer"><img src="https://img.icons8.com/color/24/000000/microsoft-excel-2019--v1.png" width="36" height="36" alt="Microsoft Excel"/> **Microsoft Excel** </a>&nbsp;
<a href="https://www.mysql.com/" target="_blank" rel="noreferrer"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="36" height="36" alt="MySQL"/> **MySQL** </a>&nbsp;
<a href="https://www.tableau.com/" target="_blank" rel="noreferrer"><img src="https://img.icons8.com/color/24/000000/tableau-software.png" width="36" height="36" alt="Tableau"/> **Tableau** </a>&nbsp;
<a href="https://powerbi.microsoft.com/" target="_blank" rel="noreferrer"><img src="https://img.icons8.com/color/24/000000/power-bi.png" width="36" height="36" alt="Power BI"/> **Power BI** </a>&nbsp;  

<a href="https://azure.microsoft.com/" target="_blank" rel="noreferrer"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/azure/azure-original.svg" width="36" height="36" alt="Microsoft Azure"/> **Microsoft Azure** </a>&nbsp;
<a href="https://cloud.google.com/" target="_blank" rel="noreferrer"><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/googlecloud/googlecloud-original.svg" width="36" height="36" alt="Google Cloud"/> **Google Cloud** </a>&nbsp;
<a href="https://colab.research.google.com/" target="_blank" rel="noreferrer"><img src="https://img.icons8.com/color/48/000000/google-colab.png" width="36" height="36" alt="Google Colab"/> **Google Colab** </a>&nbsp;&nbsp;  

---

### 🔁 Socials

<a href="https://www.github.com/tunjis/" target="_blank" rel="noreferrer">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/socials/github-dark.svg" />
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/socials/github.svg" />
    <img alt="GitHub Profile" src="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/socials/github.svg" width="32" height="32" />
  </picture>
</a>&nbsp;
<a href="https://linkedin.com/in/justincraciun/" target="_blank" rel="noreferrer">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/socials/linkedin-dark.svg" />
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/socials/linkedin.svg" />
    <img alt="LinkedIn Profile" src="https://raw.githubusercontent.com/danielcranney/readme-generator/main/public/icons/socials/linkedin.svg" width="32" height="32" />
  </picture>
</a>&nbsp;&nbsp;  

---

### ☕ Support Me

<a href="https://www.buymeacoffee.com/jstunjisu" target="_blank" rel="noreferrer"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" width="150" alt="Buy Me A Coffee"/></a>&nbsp;&nbsp;

