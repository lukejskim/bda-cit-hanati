
# Industry 4.0 의 중심, BigData

<div align='right'><font size=2 color='gray'>Data Processing Based Python @ <font color='blue'><a href='https://www.facebook.com/jskim.kr'>FB / jskim.kr</a></font>, 김진수</font></div>
<hr>

## <font color='brown'>데이터베이스, DB SQL</font>
> 
- 데이터베이스 및 테이블 생성
- 데이터 생성, INSERT
- 데이터 조회, SELECT
- 데이터 갱신, UPDATE
- 데이터 삭제, DELETE


```python
import sqlite3  

db_name = './database/my_books.db'
```
<!--
> pip install sqlite
> pip install dataset

> sqlite3 ./database/my_books.db

sqlite> .help
sqlite> .databases
sqlite> .tables
sqlite> .schema my_books
CREATE TABLE my_books (
    title text,
    published_date text,
    publisher text,
    pages integer,
    recommendation integer
);
sqlite> .quit
/-->
### 테이블 생성


```python
def create_table(db_name, db_sql):
    """
    데이터베이스 테이블을 생성하는 함수
    Args:
        db_name : Database Name
        db_sql  : Query for creating Table
    Returns : 
        is_success : Boolean 
    """
    is_success = True
    
    try :
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name)  

        # 커서 확보
        cur = conn.cursor()  

        # 테이블 생성
        cur.execute(db_sql)
    
    # except OperationalError as e:
    #     is_success = False
    #     print('Error:', e)
        
    except:
        is_success = False
        print("Database Error!")
        
    finally :        
        if is_success:
            # 데이터베이스 반영
            conn.commit()  
        else:
            # 데이터베이스 철회
            conn.rollback()
            
        # 데이터베이스 커넥션 닫기
        # print('Finish process of function.')
        conn.close()
    
    return is_success

# if __name__ == "__main__":  # 외부에서 호출 시
#     create_table()          # 테이블 생성 함수 호출

```


```python
if create_table(db_name, db_sql=None):
    print('테이블이 성공적으로 생성되었습니다.')
else :
    print('테이블이 생성되지 않았습니다.')
```

    Database Error!
    테이블이 생성되지 않았습니다.



```python
db_sql  = '''
CREATE TABLE my_books (
    title text,
    published_date text,
    publisher text,
    pages integer,
    recommendation integer
)
'''

if create_table(db_name, db_sql):
    print('테이블이 성공적으로 생성되었습니다.')
else :
    print('테이블이 생성되지 않았습니다')
```

    테이블이 성공적으로 생성되었습니다.


### 데이터 등록


```python
import sqlite3  

# 데이터 입력 함수
def insert_books(db_name):
    """
    데이터베이스 테이블에 데이터를 등록하는 함수
    Args:
        db_name : Database Name
    Returns : 
        is_success : Boolean 
    """
    is_success = True
    
    try:
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 데이터 입력 SQL1
        db_sql = "INSERT INTO my_books VALUES ('메가트랜드', '2002.03.02','A', 200, 0)"
        cur.execute(db_sql)

        # 데이터 입력 SQL2
        db_sql = 'INSERT INTO my_books VALUES (?, ?, ?, ?, ?)'
        cur.execute(db_sql, ('인더스트리 4.0', '2016.07.09','B', 584, 1))

        # # 데이터 입력 SQL3
        books = [
            ('유니콘 스타트업', '2011.07.15','A', 248, 1),
            ('빅데이터 마케팅', '2012.08.25','A', 296, 1),
            ('사물인터넷 전망', '2013.08.22','B', 526, 0)
        ]
        cur.executemany(db_sql, books)
          
    except:
        is_success = False
        print("Database Error!")
        
    finally :      
        if is_success:
            # 데이터베이스 반영
            conn.commit()  
        else:
            # 데이터베이스 철회
            conn.rollback()
            
        # 데이터베이스 커넥션 닫기
        # print('Finish process of function.')
        conn.close()
    
    return is_success    
    
# if __name__ == "__main__":          # 외부에서 호출 시
#     insert_books()                  # 데이터 입력 함수 호출

```


```python
if insert_books(db_name):
    print('데이터가 성공적으로 등록되었습니다.')
else :
    print('데이터가 등록되지 않았습니다')
```

    데이터가 성공적으로 등록되었습니다.


### 데이터 조회
title          = list()
published_date = list()
publisher      = list()
pages          = list()
recommendation = list()

column_name = ['title', 'published_date', 'publisher', 'pages', 'recommendation']
for book in books:
    # print(book)
    # for value in book:
    #     print(value, end=" | ")
    title         .append(book[0])
    published_date.append(book[1])
    publisher     .append(book[2])
    pages         .append(book[3])
    recommendation.append(book[4])
    
data = {
    'title'          : title         ,
    'published_date' : published_date,
    'publisher'      : publisher     ,
    'pages'          : pages         ,
    'recommendation' : recommendation
}

ret_df = pd.DataFrame(data, columns=column_name)
ret_df

```python
import pandas as pd

def getBooksDF(books):
    ret_df = pd.DataFrame()
    
    title          = list()
    published_date = list()
    publisher      = list()
    pages          = list()
    recommendation = list()

    column_name = ['title', 'published_date', 'publisher', 'pages', 'recommendation']
    for book in books:
        # print(book)
        # for value in book:
        #     print(value, end=" | ")
        title         .append(book[0])
        published_date.append(book[1])
        publisher     .append(book[2])
        pages         .append(book[3])
        recommendation.append(book[4])

    data = {
        'title'          : title         ,
        'published_date' : published_date,
        'publisher'      : publisher     ,
        'pages'          : pages         ,
        'recommendation' : recommendation
    }

    ret_df = pd.DataFrame(data, columns=column_name)
    
    return ret_df

```


```python
import sqlite3
import pandas as pd

def select_all_books(db_name):
    """
    전체 데이터를 조회하는 함수
    Args:
        db_name : Database Name
    Returns :
        is_success : Boolean 
        ret_df : DataFrame of books
    """
    ret_df = pd.DataFrame()
    is_success = True
    
    try:
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 조회용 SQL 실행
        db_sql = "SELECT * FROM my_books"
        cur.execute(db_sql) 

        # 조회한 데이터 불러오기
        print('[1] 전체 데이터 출력하기')
        books = cur.fetchall()                          

        ret_df = getBooksDF(books)
        
        # 데이터 출력하기
        # for book in books:                              
        #     print(book)
     
    except:
        is_success = False
        print("Database Error!")
        
    finally : 
        # 데이터베이스 커넥션 닫기
        conn.close()
        
    return is_success, ret_df


# if __name__ == "__main__":       # 외부에서 호출 시
#     select_all_books()           # 전체 조회용 함수 호출
#     print('=============================================')

```


```python
is_success, books_df = select_all_books(db_name)
if is_success:
    print('조회된 데이터는 총 %d 건 입니다.'%len(books_df))
else :
    print('데이터를 조회하지 못했습니다')

books_df
```

    [1] 전체 데이터 출력하기
    조회된 데이터는 총 5 건 입니다.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가트랜드</td>
      <td>2002.03.02</td>
      <td>A</td>
      <td>200</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>유니콘 스타트업</td>
      <td>2011.07.15</td>
      <td>A</td>
      <td>248</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>빅데이터 마케팅</td>
      <td>2012.08.25</td>
      <td>A</td>
      <td>296</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>사물인터넷 전망</td>
      <td>2013.08.22</td>
      <td>B</td>
      <td>526</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 일부 조회용 함수
def select_some_books(db_name, number):
    """
    일부 데이터를 조회하는 함수
    Args:
        db_name : Database Name
        number  : Count of data to query
    Returns : 
        is_success : Boolean 
        ret_df : DataFrame of books
    """
    ret_df = pd.DataFrame()
    is_success = True
    
    try:
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 조회용 SQL 실행
        db_sql = "SELECT * FROM my_books"
        cur.execute(db_sql) 

        # 조회한 데이터 일부 불러오기
        print('[2] 데이터 일부 출력하기')
        books = cur.fetchmany(number)                   

        ret_df = getBooksDF(books)
     
    except:
        is_success = False
        print("Database Error!")
        
    finally : 
        # 데이터베이스 커넥션 닫기
        conn.close()
        
    return is_success, ret_df                                

# if __name__ == "__main__":         # 외부에서 호출 시
#     select_some_books(3)           # 일부 조회용 함수 호출
#     print('=============================================')

```


```python
# select_some_books(db_name, number=3)

is_success, books_df = select_some_books(db_name, number=3)
if is_success:
    print('조회된 데이터는 총 %d 건 입니다.'%len(books_df))
else :
    print('데이터를 조회하지 못했습니다')

books_df

```

    [2] 데이터 일부 출력하기
    조회된 데이터는 총 3 건 입니다.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가트랜드</td>
      <td>2002.03.02</td>
      <td>A</td>
      <td>200</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>유니콘 스타트업</td>
      <td>2011.07.15</td>
      <td>A</td>
      <td>248</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 1개 조회용 함수
def select_one_book(db_name):
    """
    최상단 하나의 데이터를 조회하는 함수
    Args:
        db_name : Database Name
    Returns : 
        is_success : Boolean 
        ret_df : DataFrame of books
    """
    ret_df = pd.DataFrame()
    is_success = True
    
    try:
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 조회용 SQL 실행
        db_sql = "SELECT * FROM my_books "
        cur.execute(db_sql) 

        # 데이터 한개 출력하기
        print('[3] 1개 데이터 출력하기')
        # print(cur.fetchone())                          
        book = cur.fetchone()
        books = [book]
        ret_df = getBooksDF(books)
     
    except:
        is_success = False
        print("Database Error!")
        
    finally : 
        # 데이터베이스 커넥션 닫기
        conn.close()
        
    return is_success, ret_df                                      

# if __name__ == "__main__":        # 외부에서 호출 시
#     select_one_book()             # 1개 조회용 함수 호출
#     print('=============================================')


```


```python
# select_one_book(db_name) 

is_success, books_df = select_one_book(db_name) 
if is_success:
    print('하나의 데이터를 성공적으로 조회하였습니다.')
else :
    print('데이터를 조회하지 못했습니다')

books_df

```

    [3] 1개 데이터 출력하기
    하나의 데이터를 성공적으로 조회하였습니다.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가트랜드</td>
      <td>2002.03.02</td>
      <td>A</td>
      <td>200</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 쪽수 많은 책 조회용 함수
def find_big_books(db_name):
    """
    조건에 맞는 데이터를 조회하는 함수
    조건 : 페이지수가 300쪽보다 큰 데이터
    Args:
        db_name : Database Name
    Returns : 
        is_success : Boolean 
        ret_df : DataFrame of books
    """
    ret_df = pd.DataFrame()
    is_success = True
    
    try:
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 조회용 SQL 실행
        # db_sql = "SELECT title, pages FROM my_books "
        db_sql = "SELECT * FROM my_books "
        db_sql+= "WHERE pages > 300"
        cur.execute(db_sql) 

        # 조회한 데이터 불러오기
        print('[4] 페이지 많은 책 출력하기')
        books = cur.fetchall()
        
        ret_df = getBooksDF(books)

    except:
        is_success = False
        print("Database Error!")
        
    finally : 
        # 데이터베이스 커넥션 닫기
        conn.close()
        
    return is_success, ret_df                                   

# if __name__ == "__main__":          # 외부에서 호출 시
#     find_big_books()                # 쪽수 많은 책 조회용 함수 호출
#     print('=============================================')
```


```python
# find_big_books(db_name)

is_success, books_df = find_big_books(db_name)
if is_success:
    print('조건에 맞는 데이터는 총 %d 건 입니다.(조건:pages>300)'%len(books_df))
else :
    print('데이터를 조회하지 못했습니다')

books_df
```

    [4] 페이지 많은 책 출력하기
    조건에 맞는 데이터는 총 2 건 입니다.(조건:pages>300)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>사물인터넷 전망</td>
      <td>2013.08.22</td>
      <td>B</td>
      <td>526</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 데이터 갱신


```python
import sqlite3 

def update_books(db_name):
    """
    데이터를 수정하는 함수
    Args:
        db_name : Database Name
    Returns : 
        is_success : Boolean 
    """
    is_success = True
    
    try:
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 데이터 수정 SQL ( 제목이 ? 인 책의 추천 유무를 ? 로 변경하라 )
        db_sql = "UPDATE my_books SET recommendation=? WHERE title=? "

        # 수정 SQL 실행
        cur.execute(db_sql, (1, '메가트랜드'))

    except:
        is_success = False
        print("Database Error!")
        
    finally :      
        if is_success:
            # 데이터베이스 반영
            conn.commit()  
        else:
            # 데이터베이스 철회
            conn.rollback()
            
        # 데이터베이스 커넥션 닫기
        conn.close()
    
    return is_success   

# if __name__ == "__main__":        # 외부에서 호출 시
#     select_one_book()
#     update_books()                # 데이터 수정 함수 호출
#     print('[데이터 수정 완료] ================== ')
#     select_one_book()

```


```python
# select_one_book(db_name)
# update_books(db_name)
# print('[데이터 수정 완료] ================== ')
# select_one_book(db_name)

is_success, books_df1 = select_one_book(db_name) 

if update_books(db_name):
    print('데이터가 성공적으로 수정되었습니다.')
else :
    print('데이터가 수정되지 않았습니다')
    
is_success, books_df2 = select_one_book(db_name) 

books_df = pd.concat([books_df1, books_df2], axis=0)
books_df['update'] = ['수정전', '수정후']
books_df.set_index('update', inplace=True)
books_df

```

    [3] 1개 데이터 출력하기
    데이터가 성공적으로 수정되었습니다.
    [3] 1개 데이터 출력하기





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
    <tr>
      <th>update</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>수정전</th>
      <td>메가트랜드</td>
      <td>2002.03.02</td>
      <td>A</td>
      <td>200</td>
      <td>0</td>
    </tr>
    <tr>
      <th>수정후</th>
      <td>메가트랜드</td>
      <td>2002.03.02</td>
      <td>A</td>
      <td>200</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### 데이터 삭제


```python
import sqlite3 

# 데이터 삭제용 함수
def delete_books_by_title(db_name, title):
    """
    책제목에 해당하는 데이터를 삭제하는 함수
    Args:
        db_name : Database Name
        title   : Title of the book to be removed
    Returns : 
        is_success : Boolean 
    """
    is_success = True
    
    try:    
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  

        # 데이터 삭제 SQL
        db_sql = "DELETE FROM my_books "
        db_sql+= "WHERE title = ?      "

        # 수정 SQL 실행
        # print('db_sql:', db_sql)
        # print('title:', title)
        cur.execute(db_sql, (title,))
        # count = cur.execute(db_sql, (title,))
        # print('count:', type(count), count)
        
    except:
        is_success = False
        print("Database Error!")
        
    finally :      
        if is_success:
            # 데이터베이스 반영
            conn.commit()  
        else:
            # 데이터베이스 철회
            conn.rollback()
            
        # 데이터베이스 커넥션 닫기
        conn.close()
    
    return is_success   

```


```python
title = '메가트랜드'
if delete_books_by_title(db_name, title):
    print('데이터가 성공적으로 삭제되었습니다.')
else :
    print('데이터가 삭제되지 않았습니다')

is_success, books_df = select_all_books(db_name) 
books_df
```

    데이터가 성공적으로 삭제되었습니다.
    [1] 전체 데이터 출력하기





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>유니콘 스타트업</td>
      <td>2011.07.15</td>
      <td>A</td>
      <td>248</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>빅데이터 마케팅</td>
      <td>2012.08.25</td>
      <td>A</td>
      <td>296</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>사물인터넷 전망</td>
      <td>2013.08.22</td>
      <td>B</td>
      <td>526</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
def delete_books(db_name, col_name, col_val):
    """
    조건에 맞는 데이터를 삭제하는 함수
    Args:
        db_name  : Database Name
        col_name : Column Name
        col_val  : Column Value
    Returns : 
        is_success : Boolean 
    """
    is_success = True
    
    try: 
        # 데이터베이스 커넥션 생성
        conn = sqlite3.connect(db_name) 

        # 커서 확보
        cur = conn.cursor()  


        # 데이터 삭제 SQL
        # db_sql = "DELETE FROM my_books "
        # db_sql+= "WHERE {} = '{}' "
        # db_sql = db_sql.format(col_name, col_val)
        # cur.execute(db_sql)    

        # # 데이터 삭제 SQL
        db_sql = 'DELETE FROM my_books '
        db_sql+= 'WHERE {} = ? '
        db_sql = db_sql.format(col_name)

        # 수정 SQL 실행
        cur.execute(db_sql, (col_val,))

    except:
        is_success = False
        print("Database Error!")
        
    finally :      
        if is_success:
            # 데이터베이스 반영
            conn.commit()  
        else:
            # 데이터베이스 철회
            conn.rollback()
            
        # 데이터베이스 커넥션 닫기
        conn.close()
    
    return is_success   
    
    
# if __name__ == "__main__":     # 외부에서 호출 시
#     select_all_books()         # 테이블 전체 데이터 확인
#     delete_books()             # 데이터 삭제 함수 호출
#     print('[데이터 삭제 완료] ================== ')
#     select_all_books()         # 테이블 전체 데이터 확인

```


```python
is_success, books_df = select_all_books(db_name) 
books_df
```

    [1] 전체 데이터 출력하기





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>유니콘 스타트업</td>
      <td>2011.07.15</td>
      <td>A</td>
      <td>248</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>빅데이터 마케팅</td>
      <td>2012.08.25</td>
      <td>A</td>
      <td>296</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>사물인터넷 전망</td>
      <td>2013.08.22</td>
      <td>B</td>
      <td>526</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
col_name = 'publisher'
col_val  = 'A'
if delete_books(db_name, col_name, col_val):
    print('데이터가 성공적으로 삭제되었습니다.')
else :
    print('데이터가 삭제되지 않았습니다')

is_success, books_df = select_all_books(db_name) 
books_df
```

    데이터가 성공적으로 삭제되었습니다.
    [1] 전체 데이터 출력하기





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>사물인터넷 전망</td>
      <td>2013.08.22</td>
      <td>B</td>
      <td>526</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
col_name = 'title'
col_val  = '사물인터넷 전망'
if delete_books(db_name, col_name, col_val):
    print('데이터가 성공적으로 삭제되었습니다.')
else :
    print('데이터가 삭제되지 않았습니다')

is_success, books_df = select_all_books(db_name) 
books_df
```

    데이터가 성공적으로 삭제되었습니다.
    [1] 전체 데이터 출력하기





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>published_date</th>
      <th>publisher</th>
      <th>pages</th>
      <th>recommendation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>인더스트리 4.0</td>
      <td>2016.07.09</td>
      <td>B</td>
      <td>584</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
reset
```

    Once deleted, variables cannot be recovered. Proceed (y/[n])? 
    Nothing done.


<hr>
<marquee><font size=3 color='brown'>The BigpyCraft find the information to design valuable society with Technology & Craft.</font></marquee>
<div align='right'><font size=2 color='gray'> &lt; The End &gt; </font></div>
