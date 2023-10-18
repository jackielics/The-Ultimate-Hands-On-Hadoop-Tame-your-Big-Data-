[5. Installing Hadoop [Step by Step]](https://www.udemy.com/course/the-ultimate-hands-on-hadoop-tame-your-big-data/learn/lecture/5950274#overview)

Tutorial: 
https://hackmd.io/@firasj/BkSQJQ8eh#Lab-1---Installing-HDP-Sandbox

Download: 
[HDP 2.5.0](https://archive.cloudera.com/hwx-sandbox/hdp/hdp-2.5.0/HDP_2.5_vmware.ova)
[HDP 2.6.5](https://archive.cloudera.com/hwx-sandbox/hdp/hdp-2.6.5/HDP_2.6.5_vmware_180622.ova)
[HDP 3.0.1](https://archive.cloudera.com/hwx-sandbox/hdp/hdp-3.0.1/HDP_3.0.1_vmware_181205.ova)

Hive SQL: 
```sql
SELECT name
FROM movie_names
WHERE movie_id = 50;
```
![avatar](.\image\2023-10-17_06-38-51.png)

```sql	
SELECT movie_id, count(movie_id) as ratingCount
FROM ratings
GROUP BY movie_id
ORDER BY ratingCount
DESC;
```
![avatar](.\image\2023-10-17_07-10-16.png)

