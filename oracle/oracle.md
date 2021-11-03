### 一、单表分页查询
##### 分页查询V_COM_JZG表中时间（BGSJ）大于上次更新时间的数据
##### 上次更新时间:#{time} 页码:#{pageNo} 每页数据量:#{pageSize}
```sql
SELECT tt.*
FROM(SELECT t.*,ROWNUM AS rowno
		 FROM 
			(SELECT *
			 FROM V_COM_JZG vcj
			 WHERE to_date(vcj.BGSJ,'yyyy-MM-dd HH24:MI:SS') > to_date(#{time},'yyyy-MM-dd HH24:MI:SS')
			 ORDER BY vcj.gh) t
		 WHERE ROWNUM <= #{pageNo} * #{pageSize}) tt
WHERE tt.rowno >= (#{pageNo} - 1) * #{pageSize} + 1;
```
1. tt.rowno >= XXX为什么不放在上一行的where条件中，这样可以少包一层select?
2. 为什么先进行ORDER BY分组再做ROWNUM?






### 答案
1. 单表分页查询
   1. oracle中使用ROWNUM>的时候只能>0或者>1。因为oracle的ROWNUM做数据筛选时在一个缓冲区内，每筛选掉一条数据都会将指针下移并将新数据ROWNUM置为1。以此循环下去就会筛选掉所有数据，最终查询结果为空。[忘了就看看,虽然我现在不认可它,但是我总结不出比他好的](https://www.cnblogs.com/ikoo4396/p/7250257.html)
   2. 先进行排序是因为这是一个分页操作，需要考虑获取下一页的时候整体的数据顺序是不变的。如果先进行ROWNUM那么后面一页可能就会重复获取到前几页里的数据。