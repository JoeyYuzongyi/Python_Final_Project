# 项目背景描述：
- 青少年怀孕对母亲和儿童都是高风险，它们更有可能导致早产，低出生体重，分娩病危症和死亡。许多青春期怀孕是意料之外的，但年轻女孩可能会继续怀孕，放弃接受教育和就业的机会，或寻求不安全的堕胎方法。可以通过怀孕和分娩期间的教育和服务，安全有效的避孕措施以及性传播疾病的预防和治疗来提供支持，收入和中等收入国家中，怀孕和分娩的病发症是导致育龄妇女和致残的主要原因。
# github仓库
[github仓库](https://github.com/JoeyYuzongyi/Python_Final_Project)
## 我们的网站一共有3个URL
# - pythonanywhere URL
- http://joeyyuzongyi.pythonanywhere.com/
- http://joeyyuzongyi.pythonanywhere.com/search
- http://joeyyuzongyi.pythonanywhere.com/story
- http://joeyyuzongyi.pythonanywhere.com/new
# - 数据传递描述
## python档描述
```
@app.route('/new', methods=['GET'])
def new():
    return app.send_static_file('new.html')
```
## 第2个修饰符建立“/new”URL，new()调用app.send_static_file，然后将结果new.html返回

```
@app.route('/story', methods=['GET'])
def story():
    return render_template('story.html')
```
## 第3个修饰符建立“/story”URL，story()调用render_template，然后将结果story.html返回

![app1](https://github.com/JoeyYuzongyi/Python_Final_Project/blob/master/app1.png)

```
@app.route('/search',methods=['POST'])
def search():
    dfa = pd.read_csv('./data/data.csv', encoding='gbk')
    area = request.form.to_dict()['area']
    areaData = dfa.query("CountryName=='{}'".format(area)).sort_values(by="Year" , ascending=True).to_dict('records')
    year = []
    gdp = []
    rate = []
    for i in areaData:
        year.append(i['Year'])
        if i['IndicatorName'] == 'GDP':
            gdp.append(i['Value'])
        else:
            rate.append(i['Value'])
    year = list(set(year))
    year.sort()
    bar = Bar()
    bar.add_xaxis(year)
    bar.add_yaxis(area,rate)
    bar.add_yaxis(area,gdp)
    bar.set_global_opts(title_opts=opts.TitleOpts(title=area+"青少年生育率/国民生产总值GDP"))
    bar.render('./templates/search.html')
    return render_template('search.html')
```
## 第4个修饰符建立“/search”URL，search()调用render_template，然后将结果search.html返回

![app2](https://github.com/JoeyYuzongyi/Python_Final_Project/blob/master/app2.png)

```
@app.route('/afr', methods=['GET'])
def arf():
    dfa = pd.read_csv('./data/Adolescent_fertility_rate.csv', encoding='utf8')
    dfa1 = dfa.dropna(axis=0, how='any')

def timeline_map() -> Timeline:
    tl = Timeline()
    for i in range(2010, 2017):
        map0 = (
            Map()
                .add(
                "青少年怀孕率", (list(zip(list(dfa1.CountryName), list(dfa1["{}".format(i)])))), "world",
                is_map_symbol_show=False
            )
                .set_global_opts(
                title_opts=opts.TitleOpts(title="".format(i), subtitle="",
                                          subtitle_textstyle_opts=opts.TextStyleOpts(color="red", font_size=16,
                                                                                     font_style="italic")),
                visualmap_opts=opts.VisualMapOpts(min_=0, max_=100, series_index=0),

            )
        )
        tl.add(map0, "{}".format(i))
    return tl
timeline_map().render('./templates/afr.html')
return render_template('afr.html')
```
## 第5个修饰符建立“/afr”URL，timeline_map调用render_template，然后将结果afr.html返回
## 第6个修饰符建立“/gdp”URL，timeline_map调用render_template，然后将结果gdp.html返回
## 第7个修饰符建立“/gdp_afp”URL，timeline_map调用render_template，然后将结果gdp_afp.html返回
## 第8个修饰符建立“/min_afr”URL，timeline_map调用render_template，然后将结果min_afr.html返回
## 第9个修饰符建立“/max_afr”URL，timeline_map调用render_template，然后将结果max_afr.html返回
- 为呈现HTML表单，我对代码做出以下修改：
   
1.1 导入render_template
`from flask import Flask, render_template`

1.2 创建url，这里是“/”，方法是GET
`@app.route('/', methods=['GET'])`

1.3 创建一个函数返回正确呈现的HTML
```
@app.route('/', methods=['GET'])
def index():
    dfa = pd.read_csv('./data/data.csv', encoding='gbk')
    data_str = dfa.to_html()
    return render_template('index.html',the_res=data_str,the_select_region=regions_available)
```
## “index.html”是要呈现的模板名

# - 我为了改进编辑，将最后一行代码修改如下：app.run(debug=True, port=8000)
# 在web应用中使用请求数据
>  from flask import Flask, render_template, request
## 我使用了pip安装flask微web框架，然后用它来构建我的web项目，并且了解如何使用jinja2在web应用呈现html页面

## Webapp动作描述
- 我们将要创建的应用程序将依赖于几个不同的文件，因此，我们需要做的第一件事是创建一个项目文件夹来保存所有这些文件。我创建的项目文件夹名字为project，主要运行文件为app.py
- 网站首页显示有世界各国下拉框，单击do it按钮将数据发送到web服务器，筛选不同国家青少年生育率/国民生产总值GDP之比，首页还显示data/min_AFR.csv表单。点击”数据故事“跳转到/story，一共展示了8个图表，分别是
   
2.1 2010年到2016年全球青少年怀孕率

2.2 2010年到2016年全球青少年怀孕率和人均GDP分析

2.3 青少年怀孕率最高8国GDP

2.4 Finland和Mal从1990年到2015年的政府教育投入资金的趋势图

2.5 2010年到2016年人均GDP的全球分布轮播图

2.6 2010-1017全球青少年怀孕率地图

2.7 2014-2017全球最高青少年怀孕率8国数据

2.8 2014-2017全球最低青少年怀孕率4国数据

- 首页还有一个跳转链接是dash仪表板。我使用的用户界面层仪表板是由JavaScript。CSS和HTML组合。我已经在应用程序中使用了一些注释性代码来存储将作为图表输入的值。


## HTML档描述
- 该文件夹中的html文件是17级制作的交互式可视化数据图，我们利用jupyter将每个数据图分别导出为html格式，同时为了在python文档中的路径跳转做准备.
- 一共有10个HTML，分别是
   
3.1 afr.html全球青少年怀孕率

3.2 example1.html Aurba2010-2017人均GDP

3.3 gdp.html全球人均GDP

3.4 gdp_afp青少年怀孕率最高八国生育率和人均GDP

3.5 index.html web服务器的“餐巾纸草图”，提交的数据会返回给用户

3.6 max_afr.html青少年怀孕率条形图

3.7 min_afr.html两国教育程度对比图

3.8 search.html青少年生育率/国民生产总值GDP

3.9 story.html 数据故事

- 该项目的总结和思考
青少年生育率高的国家普遍分布在非洲地区，这些国家的人均GDP相对较低，在世界分区中属于落后国家的行列，而这些国家政府在教育上又投资的多少，在教育方面是否存在一定的缺失。 过早的怀孕对少女来说还不只是健康的问题，即使她们顺利生下了孩子，也要面对社会的各种压力。 
