# ChinesePoetryLibrary
收录超80w中国古诗文，进行了AI鉴赏、去重处理，统一数据格式，并提供4096维度的向量数据，以json格式给出

## 参数格式

```json
{
    "id": "int", // 在所在目录下唯一
    "poetry_id": "int", // 全局唯一
    "author": "string", // 作者，提供作者表，姓名在作者表内唯一
    "title": "string", // 标题
    "content": "string", // 正文
    "translation": "string", // 译文
    "apprecation": "string", // 赏析
    "explanation1": [
        {
            "word": "string", // 字词,
            "meaning": "string" // 注释
        }
    ], // 一版本注释
    "explanation2": [
        {
            "word": "string", // 字词,
            "meaning": "string" // 注释
        }
    ], // 版本二注释
    "tags": [], // 标签
    "allusions": [
        {
            "word": "string", // 词语
            "allusion": "string", //典故
            "source": "string" // 出处
        }
    ], // 典故
    "interpret": "string", //讲解 
    "comment": "string" // 评论
}
```



## 数据量

数据持续更新中... 

`/`后为向量化数据个数，部分数据过长无法向量化，在云盘中有标注id（非poetryId），`*` 表示所有数据都已向量化

- 唐：55386 /  *

- 宋：
  - 宋诗：249534 /  *
  - 宋词：20026 /  *
  
- 明：249761 / 249756

- 清：115856 / 115855

- 元：51709 / *

- 其他：72971 / 72970

- 古文观止：222 / 0

- 总计：815465

  

## 目录说明

按朝代进行分类，本项目提供了向量化数据，维度为`4096` 维度，由于数据量过大，进行了切片处理，格式为

```json
{
    "id": "int", // 在所在目录下唯一
    "poetry_id":"int", // 全局唯一
    "embedding": [], // 向量化数据，数据类型为float，4096项
    ...
}
```

向量链接

阿里云盘链接：https://www.alipan.com/s/s4X5PAYJ5qB

huggingface链接：https://huggingface.co/datasets/byj233/ChinesePoetry-embedding



## 数据来源

正文数据来源

https://github.com/javayhu/poetry

https://github.com/chinese-poetry/chinese-poetry

只收集了标题、正文等

去重说明：

确保所有数据中，正文数据没有相同项

通过移除非中文字符，比较`content`确保同一个朝代没有重复项

> 补录内容仅进行简单去重



## 开源协议

`Mozilla Public License Version 2.0`仅允许在学习过程中使用，若对以上数据进行二次开发，需要将成果开源，若商用，请联系作者`获取授权`，违者后果自负

