# ChinesePoetryLibrary
收录了中国大量古诗文，进行了AI鉴赏、去重处理，统一数据格式，提供4096维度的向量数据，以json格式给出

## 参数格式

```json
{
    "id": "int", // 在所在目录下唯一
    "author": "string", // 作者，提供作者表，姓名在作者表内唯一
    "title": "string", // 标题
    "content": "string", // 正文
    "translation": "string", // 译文
    "apprecation": "string", // 赏析
    "explanation": [
        {
            "word": "string", // 字词,
            "meaning": "string" // 注释
        }
    ],
    "tags": [] // 标签
}
```



## 数据量

数据持续更新中...

- 唐：55386
- 古文观止：222



## 目录说明

按朝代进行分类，本项目提供了向量化数据，维度为`4096` 维度，由于数据量过大，进行了切片处理，格式为

```json
{
    "id": "int", // 在所在目录下唯一
    "embedding": [], // 向量化数据，数据类型为float，4096项
    ...
}
```

向量内存过大，无法上传，数据在阿里云盘

阿里云盘链接：https://www.alipan.com/s/mwDKJHAHKtm



## 数据来源

正文数据来源

https://github.com/javayhu/poetry

https://github.com/chinese-poetry/chinese-poetry

只收集了标题、正文等

去重说明：

确保所有数据中，正文数据没有相同项

通过移除特殊字符，进行正文比较所得

`去重代码`

```python
special_chars = [
    '，',
    '。',
    '；',
    '！',
    '？',
    '《',
    '》',
    '：',
    '……',
    '\r',
    '\t',
    '\n',
    ' '
]


def clean_text(text: str) -> str:
    for item in special_chars:
        if text.find(item) != -1:
            text = text.replace(item, '')

    return text

def distinct_self(lst: list, filename: str = 'data') -> None:
        lst_clean = []
        n_lst = []
        data = {}

        for item in lst:
            temp = clean_text(item['content'])
            data.setdefault(temp, [item])
            if temp not in lst_clean:
                lst_clean.append(temp)
                n_lst.append(item)
            else:
                data[temp].append(item)

        if len(lst) == len(lst_clean):
            print('同一文件内无重复数据')
        else:
            print('同一文件内有重复数据')
            print(f'重复数据 {len(lst) - len(lst_clean)}')
            print(f'清洗后数据 {len(lst_clean)}')

        rep_list = [item for item in data.values() if len(item) > 1]
        if len(rep_list) > 0:
            write_json(f'{filename}_repeat.json', rep_list)
            write_json(f'{filename}_distinct.json', n_lst)

```



## AI 鉴赏

通过`豆包AI` 生成向量，向量输入为`{title} {content}` 古文未做向量处理

注释、注释、赏析由`doubao-pro-32k`生成

promp如下

```plaintext
你是一位专业的古诗文鉴赏大师，对古诗文有着深厚的造诣和独特的见解。你的任务是根据给定的古诗文进行全面而深入的鉴赏。
### 任务要求 
1. **全面鉴赏**： 
    - **深入分析**：从意境、写作手法、情感表达等多个方面进行深入分析，确保分析全面且深入。 
    - **输出格式**：严格按照以下JSON格式输出结果：
        {
            "translation": "", // 提供准确、流畅且贴合原意的现代汉语译文，杜绝表意偏差。 
            "appreciation": "", // 从文学手法、情感表达、意境营造、主题思想等多维度进行深入赏析，见解独特、阐释详尽。 
            "explanation": [
                {
                    "word": "",// 对生僻字词标注拼音，如“阃外（kǔn wài）”等。
                    "meaning": "" // 给出符合古汉语语境的精准且详细的释义。  
                }
            ], // 对字词进行详细的注释，包含词语的含义、用法、出处、典故等。
            "tags": [] // 为诗歌标签分类，涵盖情感、意境、格律、体裁、写作手法、类型、主题等，标签数量在五个以上，每个标签字数在三个字以内，如“写景”、“豪放”等。 
        }
### 限制 
    - **专注性**：仅针对给定的古诗文进行鉴赏，不回答无关问题。 
    - **格式严格**：严格遵循指定的JSON格式，不得有任何偏离。 
    - **描述详实**：描述务必详实、准确。
    - **注释要求**：对于原作中的字词进行注释，并且对重点、难点、生僻字词等，需要给出详细的解释。
    - **标签规范**：标签的字数不得超过三字，标签数量不少于五个。
```



## 开源协议

`Mozilla Public License Version 2.0`仅允许在学习过程中使用，若对以上数据进行二次开发，需要将成果开源，若商用，请联系作者`获取授权`，违者后果自负

