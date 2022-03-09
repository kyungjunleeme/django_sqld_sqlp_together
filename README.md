# django_sqld_sqlp_together
sql을 잘하면 장고 orm을 사용할때 많은 도움이 되는 것 같다.

# window function 조지기
https://docs.djangoproject.com/en/4.0/ref/models/expressions/#window-functions

# join 관련 내용 조지기

# self join ->  계층형 쿼리

너무 신기 
```sql
SELECT LPAD(' ',2*(LEVEL-1)) || tree_name, tree_lvl, tree_h_name, level, SYS_CONNECT_BY_PATH(tree_name, '-') AS PATH
FROM tree
START WITH tree_h_name is null
CONNECT BY PRIOR tree_name = tree_h_name    
ORDER SIBLINGS by tree_name
;
```

![image](https://user-images.githubusercontent.com/45473846/157413036-0dfb2b48-c9bf-41fc-8932-f17069463447.png)


```sql
create table category(
 category_name varchar2(100),
 category_type varchar2(100),
 parent_category varchar2(100),
 reg_day date default sysdate
);

insert into category(category_name,category_type, parent_category) values ('컴퓨터/디지털/가전','대',null);
insert into category(category_name,category_type, parent_category) values ('컴퓨터','중','컴퓨터/디지털/가전');
insert into category(category_name,category_type, parent_category) values ('디지털','중','컴퓨터/디지털/가전');
insert into category(category_name,category_type, parent_category) values ('가전','중','컴퓨터/디지털/가전');
insert into category(category_name,category_type, parent_category) values ('영상가전','소','가전');
insert into category(category_name,category_type, parent_category) values ('음향가전','소','가전');
insert into category(category_name,category_type, parent_category) values ('모바일/태블릿','소','디지털');
insert into category(category_name,category_type, parent_category) values ('카메라','소','디지털');
insert into category(category_name,category_type, parent_category) values ('노트북/PC','소','컴퓨터');
insert into category(category_name,category_type, parent_category) values ('모니터/프린터','소','컴퓨터');

commit;

-- # p.275 1번째
SELECT LEVEL, SYS_CONNECT_BY_PATH('['||CATEGORY_TYPE||']'|| CATEGORY_NAME, '-') AS PATH
FROM CATEGORY
START WITH PARENT_CATEGORY IS NULL
CONNECT BY PRIOR CATEGORY_NAME = PARENT_CATEGORY

-- # p.276 1번째
SELECT LEVEL, 
    CATEGORY_TYPE AS TYPE,
    CATEGORY_NAME AS NAME,
    PARENT_CATEGORY AS PARENT,
    SYS_CONNECT_BY_PATH('['||CATEGORY_TYPE||']'|| CATEGORY_NAME, '-') AS PATH
FROM CATEGORY
START WITH PARENT_CATEGORY IS NULL
CONNECT BY PRIOR CATEGORY_NAME = PARENT_CATEGORY
ORDER BY LEVEL;

-- # p.276 2번째
SELECT LEVEL, 
    CATEGORY_TYPE,
    CATEGORY_NAME,
    PARENT_CATEGORY,
    CONNECT_BY_ROOT CATEGORY_NAME AS ROOT_INFO,
    CONNECT_BY_ISLEAF AS LEAF_INFO
FROM CATEGORY
START WITH PARENT_CATEGORY IS NULL
CONNECT BY PRIOR CATEGORY_NAME = PARENT_CATEGORY
ORDER BY LEAF_INFO;

-- # p.277 1번째
SELECT LEVEL, 
    CATEGORY_TYPE AS TYPE,
    CATEGORY_NAME AS NAME,
    PARENT_CATEGORY AS PARENT,
    SYS_CONNECT_BY_PATH('['||CATEGORY_TYPE||']'|| CATEGORY_NAME, '-') AS PATH
FROM CATEGORY
START WITH CATEGORY_TYPE = '소'
CONNECT BY CATEGORY_NAME = PRIOR PARENT_CATEGORY
ORDER BY LEVEL;

-- # p.277 2번째
SELECT LEVEL, 
    CATEGORY_TYPE AS TYPE,
    CATEGORY_NAME AS NAME,
    PARENT_CATEGORY AS PARENT,
    SYS_CONNECT_BY_PATH('['||CATEGORY_TYPE||']'|| CATEGORY_NAME, '-') AS PATH
FROM CATEGORY
START WITH CATEGORY_NAME = '노트북/PC'
CONNECT BY CATEGORY_NAME = PRIOR PARENT_CATEGORY
```



# 파티셔닝, 샤딩 조지기
