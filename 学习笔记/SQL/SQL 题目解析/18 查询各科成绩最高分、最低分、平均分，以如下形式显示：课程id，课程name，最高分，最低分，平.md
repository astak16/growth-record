# 18 查询各科成绩最高分、最低分、平均分，以如下形式显示：课程id，课程name，最高分，最低分，平均分，及格率，中等率，优良率，及格≥60，中等70~80，优良80~90，优秀≥90

## SQL

```sql
select 
	sc.c_id, 
	c_name, 
	max(score) as 最高分,
	min(score) as 最低分,
	avg(score) as 平均分,
	(100 * (sum(case when score >= 60 then 1 else 0 end) / sum(case when score then 1 else 0 end))) as 及格率,
	(100 * (sum(case when score >= 70 and score <= 80 then 1 else 0 end) / sum(case when score then 1 else 0 end))) as 中等率,
	(100 * (sum(case when score >= 80 and score <= 90 then 1 else 0 end) / sum(case when score then 1 else 0 end))) as 优良率,
	(100 * (sum(case when score >= 90 then 1 else 0 end) / sum(case when score then 1 else 0 end))) as 优秀率
from sc 
left join course on sc.c_id = course.c_id 
group by sc.c_id, c_name;
```

## 解析

- 使用 `case ... when ... then ... else ... end` 的语法查询出各个范围。
- 当在要查询的区间内，输出 `1` 否者输出 `0`
- 使用 `sum` 函数算出有多少个 `1`
- 用 `case ... when ... then ... else ... end` 查找出有值的 `score`
- 如果 `score` 有值，输出为 `1` 否者输出 `0`
- 使用 `sum` 函数算出有多少个 `1`
- 最后两个相比就能算出各区间的百分比了