---
title: django使用sqlite3统计前台站点访问数量
sidebar:
  - toc
date: 2023-12-20 17:15:08
tags:
---

# Django统计前台站点访问数量

## 后端
Django框架自带sqlite3数据库，通过models.py文件内定义数据库类用来存储访问信息
```
from django.db import models
import time

# 网站总访问次数
class VisitNumber(models.Model):
    date = models.IntegerField(verbose_name='网站访问时间', default=int(time.time()))

    class Meta:
        verbose_name = '网站访问时间'
        verbose_name_plural = verbose_name

    def __str__(self):
        return str(self.count)
```

新增接口，用以向数据库中新增一条数据，前台页面渲染过程中调用一次该接口
```
def set_visit_count(request):
    cur_time = int(time.time())
    count_nums = VisitNumber()
    count_nums.date = cur_time
    count_nums.save()
    return JsonResponse({"status": True, "data": cur_time})

```
新增接口，用以从数据库获取访问信息
```
conn = sqlite3.connect("/root/Django/db.sqlite3")
cursor = conn.cursor()
data = cursor.execute(f"select * from DeployProductRepair_visitnumber where date > {start_time} and date < {end_time};")
conn.commit()
cur_time_visit = len(data.fetchall())
cursor.close()
conn.close()
return cur_time_visit
```
因为models.py文件发生改变，所以需要在环境上重新生成数据库表，执行以下两行命令，之后启动python服务
```
python3 /root/Django/manage.py makemigrations

python3 /root/Django/manage.py migrate

nohup python3 /root/Django/manage.py runserver 10.243.22.192:8080
```
## 前端
src/api/index.ts文件中增加一个请求
```
const setVisitCount = (params) => {
  return request.get("/setVisitCount/", params) as Promise<{
    status: boolean;
    data: Array<any>;
  }>;
}

```
在总入口文件app.tsx中增加调用请求
```
import { setVisitCount } from "./api/index.ts";

//并不需要对返回值做处理，因此可以不必使用await同步
const visitCount = setVisitCount(null);
```
在需要显示数据的页面调用获取请求并渲染即可
```
const { data, count } = await getUseData({ type: this.state.type });
this.setState({
    visitCount: count,
});
```
