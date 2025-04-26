## Library Management System Report

### Overview

This project analyzes the "Goodreads Books" dataset from Kaggle (`zygmunt/goodbooks-10k`), licensed under CC-BY-SA-4.0, to simulate a library management system.<br>
The dataset includes ~10,000 records of books (`books.csv`) with `book_id`, `title`, `authors`, `original_publication_year`, `average_rating`, and `ratings_count`, supplemented by genre data from `book_tags.csv`.<br>
The goal was to extract advanced insights using complex SQL queries, showcasing basic syntax, querying techniques, and database concepts.<br>
The analysis was conducted in Google Colab using SQLite, sampling up to 10,000 rows for efficiency.

### Dataset Description

The dataset includes:<br>
- **Books**: `book_id` (PRIMARY KEY), `title` (NOT NULL), `authors`, `original_publication_year`, `average_rating`, `ratings_count`.<br>
- **BookTags**: `goodreads_book_id` (FOREIGN KEY to `book_id`), `tag_id`, `count` (genre association strength).<br>

A SQLite schema was created with relational constraints to support joins and complex queries.

### SQL Techniques Demonstrated

The project showcases:<br>
- **Basic SQL Syntax**: `INSERT` (new book), `UPDATE` (ratings), `DELETE` (low-rated books), `SELECT`.<br>
- **Basic Queries**: Retrieve (Query 4), Filter (Query 3), Manipulate (Query 5 with `ROW_NUMBER`).<br>
- **Database Concepts**: `PRIMARY KEY`, `FOREIGN KEY`, joins, grouping.<br>
- **Complex Queries**: Subqueries (Query 4), CTEs and window functions (Queries 5, 6).<br>
### Analysis and Insights

The project executed six SQL queries, with Queries 4–6 using advanced techniques. Below are the queries, results, and insights.

#### Query 1: Most Popular Authors by Ratings

**Objective**: Identify top authors by total ratings.<br>
**SQL Techniques**: `SELECT`, `SUM`, `GROUP BY`, `ORDER BY`, `WHERE`, `LIMIT`.<br>
**Results**:
```
                        authors  total_ratings
0  J.K. Rowling, Mary GrandPrÃ©       13372767
1               Suzanne Collins        8646393
2               Stephenie Meyer        8403438
3                  Stephen King        6505240
4                J.R.R. Tolkien        5262785
```
**Insights**:<br>
- J.K. Rowling leads with 13,372,767 ratings, driven by the Harry Potter series.<br>
- Fantasy and young adult genres dominate the top authors.<br>
- This highlights a strong demand for popular fiction.<br>
**Business Implication**: Promote top authors’ works.

#### Query 2: Books by Publication Decade

**Objective**: Analyze book distribution and ratings by decade.<br>
**SQL Techniques**: `SELECT`, `(year / 10) * 10`, `COUNT`, `SUM`, `GROUP BY`, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
   decade  book_count  total_ratings
0    2020           1            100
1    2010        3067      120581462
2    2000        3121      176212429
3    1990        1360       73491621
4    1980         704       34352837
```
**Insights**:<br>
- The 2000s peak with 3,121 books and 176,212,429 ratings.<br>
- Recent decades (2010s, 2000s) show high engagement.<br>
- Older decades maintain significant ratings.<br>
**Business Implication**: Balance new releases with classic retention.

#### Query 3: Authors with Multiple Highly Rated Books

**Objective**: Identify authors with >5 books rated 4.0+.<br>
**SQL Techniques**: `SELECT`, `COUNT`, `GROUP BY`, `HAVING`, `WHERE`, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
           authors  book_count
0     Nora Roberts          44
1        J.D. Robb          33
2  Terry Pratchett          32
3    John Sandford          27
4   Kristen Ashley          26
```
**Insights**:<br>
- Nora Roberts leads with 44 highly rated books, excelling in romance.<br>
- Consistent quality across genres (romance, fantasy).<br>
- Suggests a robust mid-tier author base.<br>
**Business Implication**: Target marketing to these authors’ fans.

#### Query 4: Top Books by Genre Popularity with Subquery

**Objective**: Find books with above-average ratings and genre popularity.<br>
**SQL Techniques**: `SELECT`, `JOIN`, `WHERE`, subquery, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
                                               title  \
0  Harry Potter and the Deathly Hallows (Harry Po...   
1  Harry Potter and the Deathly Hallows (Harry Po...   
2             Queen of Shadows (Throne of Glass, #4)   
3  Harry Potter and the Half-Blood Prince (Harry ...   
4  A Court of Wings and Ruin (A Court of Thorns a...   

                        authors  average_rating  ratings_count  tag_id  \
0  J.K. Rowling, Mary GrandPrÃ©            4.61        1746574   11574   
1  J.K. Rowling, Mary GrandPrÃ©            4.61        1746574   32989   
2                 Sarah J. Maas            4.60          99067   30574   
3  J.K. Rowling, Mary GrandPrÃ©            4.54        1678823    2106   
4                 Sarah J. Maas            4.54          55037   33114   

   tag_count  
0        369  
1       4481  
2       3078  
3        305  
4        943  
```
**Insights**:<br>
- Harry Potter books dominate with high ratings (4.61, 4.54) and tag counts.<br>
- Genre tags (e.g., 32989: fantasy) correlate with popularity.<br>
- Fantasy and young adult genres are key drivers.<br>
**Business Implication**: Expand fantasy collections.

#### Query 5: Author Consistency with CTE and Window Function

**Objective**: Assess author rating consistency.<br>
**SQL Techniques**: `WITH`, `SELECT`, `ROW_NUMBER`, `AVG`, `MAX`, `SUM`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
                           authors  total_books  avg_rating  top_rating  \
0                   Bill Watterson           12    4.710833        4.82   
1     J.K. Rowling, Mary GrandPrÃ©            8    4.547500        4.77   
2                    Sarah J. Maas           13    4.453077        4.72   
3  Brian K. Vaughan, Fiona Staples            7    4.418571        4.57   
4                  Cassandra Clare           13    4.400769        4.59   

   highly_rated_count  
0                  12  
1                   8  
2                  13  
3                   7  
4                  13  
```
**Insights**:<br>
- Bill Watterson achieves a 4.71 average with 12 highly rated books.<br>
- Sarah J. Maas and Cassandra Clare show consistency with 13 books each.<br>
- High `highly_rated_count` indicates reliable quality.<br>
**Business Implication**: Feature consistent authors.

#### Query 6: Cumulative Ratings Trend by Decade

**Objective**: Track cumulative rating growth.<br>
**SQL Techniques**: `SELECT`, subquery, `SUM OVER`, `ORDER BY`, `LIMIT`.<br>
**Results**:
```
   decade  book_count  total_ratings  cumulative_ratings
0    2020           1            100           539328682
1    2010        3067      120581462           539328582
2    2000        3121      176212429           418747120
3    1990        1360       73491621           242534691
4    1980         704       34352837           169043070
```
**Insights**:<br>
- Cumulative ratings reach 539,328,682 by 2020, showing massive growth.<br>
- The 2000s contribute 176,212,429 ratings, a significant jump.<br>
- Steady increase from 1980s onward.<br>
**Business Implication**: Invest in recent decades.



### Conclusion

This Library Management System offers advanced insights:<br>
- **Collection Strategy**: Promote top authors (Rowling) and consistent performers (Watterson).<br>
- **Genre Focus**: Expand fantasy based on genre popularity.<br>
- **Historical Trends**: Leverage 2000s growth and cumulative rating data.<br>

Future analyses could:<br>
- Integrate `ratings.csv` for user trends.<br>
- Analyze tag diversity with `tags.csv`.<br>
- Predict future popularity with rating patterns.<br>

This project highlights SQL mastery and strategic value.
