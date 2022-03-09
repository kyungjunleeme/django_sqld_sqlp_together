# django_sqld_sqlp_together
sql을 잘하면 장고 orm을 사용할때 많은 도움이 되는 것 같다.

# window function 조지기
https://docs.djangoproject.com/en/4.0/ref/models/expressions/#window-functions

# join 관련 내용 조지기

# 계층형 쿼리

너무 신기 
```sql
SELECT  LPAD(' ',2*(LEVEL-1)) || tree_name, tree_lvl, tree_h_name,level
FROM tree
START WITH tree_h_name is null
CONNECT BY PRIOR tree_name = tree_h_name    
ORDER SIBLINGS by tree_name
;
```


![image](https://user-images.githubusercontent.com/45473846/157410030-da590a29-7b74-4077-86ea-0ac73e8773dd.png)



# 파티셔닝, 샤딩 조지기
