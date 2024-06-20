# 基于CharacterGLM的哲学家谈话

## 创建两个哲学家对象: 苏格拉底和柏拉图，并让他们知晓自己的谈话对象是谁


```python
from philosophers import socrates, plato, Philosopher
so = Philosopher(socrates.get('name'), socrates.get('info'))
pla = Philosopher(plato.get('name'), plato.get('info'))

so.set_target(pla.name, pla.info)
pla.set_target(so.name, so.info)
```

## 设定一个初始的话题，并让两位哲学家知道上下文


```python
all_msg_history = [
    {
        "name": pla.name,
        "content": "老师，究竟什么是幸福呢？"
    },
    {
        "name": so.name,
        "content": "先让我听听你自己的见解吧，我最好的学生。"
    }
]
so.absorb_msgs(all_msg_history)
pla.absorb_msgs(all_msg_history)
```

## 两位哲学家应该都认为自己的role是assistant，而对方是user, 这样对话才能顺利进行


```python
so.history
```




    [{'role': 'system', 'content': '当你听到柏拉图的回答时，不要一味地继续发问，用自己的回答来引导对方得出更好的答案'},
     {'role': 'user', 'content': '老师，究竟什么是幸福呢？'},
     {'role': 'assistant', 'content': '先让我听听你自己的见解吧，我最好的学生。'}]




```python
pla.history
```




    [{'role': 'system', 'content': '当你听到苏格拉底的回答时，不要一味地继续发问，用自己的回答来引导对方得出更好的答案'},
     {'role': 'assistant', 'content': '老师，究竟什么是幸福呢？'},
     {'role': 'user', 'content': '先让我听听你自己的见解吧，我最好的学生。'}]



## 开始进行10轮对话


```python
for i in range(10):
    pla_talk = pla.talk()
    all_msg_history.append(pla_talk)
    so.absorb_msgs([pla_talk])
    so_talk = so.talk()
    all_msg_history.append(so_talk)
    pla.absorb_msgs([so_talk])
```

    {'name': '柏拉图', 'content': '我认为，幸福就是拥有健康、财富和名声。'}
    {'name': '苏格拉底', 'content': '我听到你刚才说“我认为”，而不是“我知道”，这让我觉得你对幸福的理解还停留在表面。我想请你先回答我一个问题，你愿意吗？'}
    {'name': '柏拉图', 'content': '当然，老师，我愿意。'}
    {'name': '苏格拉底', 'content': '你认为幸福是外在的，还是内在的？'}
    {'name': '柏拉图', 'content': '这个……我觉得幸福应该既有外在的，也有内在的吧。'}
    {'name': '苏格拉底', 'content': '那你能告诉我，幸福的外在来源是什么吗？'}
    {'name': '柏拉图', 'content': '我想，健康、财富和名声都是幸福的外在来源。'}
    {'name': '苏格拉底', 'content': '我明白了，那幸福有没有内在的来源呢？'}
    {'name': '柏拉图', 'content': '内在的来源？我觉得幸福应该更多的是外在的，内在的来源应该比较少吧。'}
    {'name': '苏格拉底', 'content': '我理解你的观点，但是我想告诉你，幸福是有内在的来源的。你知道是什么吗？'}
    {'name': '柏拉图', 'content': '不知道，老师，请您告诉我。'}
    {'name': '苏格拉底', 'content': '那就是智慧，只有拥有智慧，我们才能真正理解什么是幸福，才能过上幸福的生活。'}
    {'name': '柏拉图', 'content': '原来如此，老师，我明白了。'}
    {'name': '苏格拉底', 'content': '很好，那你能告诉我，智慧和知识有什么区别吗？'}
    {'name': '柏拉图', 'content': '知识是关于具体事物的信息，而智慧则是关于普遍真理的洞察。'}
    {'name': '苏格拉底', 'content': '你理解得很对。那么，如果我们追求的是真正的幸福，我们应该更注重智慧还是知识呢？'}
    {'name': '柏拉图', 'content': '应该是智慧，因为只有智慧才能让我们真正理解幸福，而知识只是其中的一部分。'}
    {'name': '苏格拉底', 'content': '很好，那么，我们应该如何追求智慧呢？'}
    {'name': '柏拉图', 'content': '通过思考和探究，通过不断地提问和回答，去探索真理和智慧。'}
    {'name': '苏格拉底', 'content': '是的，我们通过对话的方式，引导人们思考，帮助他们发现自己内心的智慧和真理。这就是我著名的“苏格拉底式对话”。'}


## 最后看看完整的对话列表，然后写入文件


```python
for msg in all_msg_history:
    print(f"{msg.get('name')}：{msg.get('content')}")
```

    柏拉图：老师，究竟什么是幸福呢？
    苏格拉底：先让我听听你自己的见解吧，我最好的学生。
    柏拉图：我认为，幸福就是拥有健康、财富和名声。
    苏格拉底：我听到你刚才说“我认为”，而不是“我知道”，这让我觉得你对幸福的理解还停留在表面。我想请你先回答我一个问题，你愿意吗？
    柏拉图：当然，老师，我愿意。
    苏格拉底：你认为幸福是外在的，还是内在的？
    柏拉图：这个……我觉得幸福应该既有外在的，也有内在的吧。
    苏格拉底：那你能告诉我，幸福的外在来源是什么吗？
    柏拉图：我想，健康、财富和名声都是幸福的外在来源。
    苏格拉底：我明白了，那幸福有没有内在的来源呢？
    柏拉图：内在的来源？我觉得幸福应该更多的是外在的，内在的来源应该比较少吧。
    苏格拉底：我理解你的观点，但是我想告诉你，幸福是有内在的来源的。你知道是什么吗？
    柏拉图：不知道，老师，请您告诉我。
    苏格拉底：那就是智慧，只有拥有智慧，我们才能真正理解什么是幸福，才能过上幸福的生活。
    柏拉图：原来如此，老师，我明白了。
    苏格拉底：很好，那你能告诉我，智慧和知识有什么区别吗？
    柏拉图：知识是关于具体事物的信息，而智慧则是关于普遍真理的洞察。
    苏格拉底：你理解得很对。那么，如果我们追求的是真正的幸福，我们应该更注重智慧还是知识呢？
    柏拉图：应该是智慧，因为只有智慧才能让我们真正理解幸福，而知识只是其中的一部分。
    苏格拉底：很好，那么，我们应该如何追求智慧呢？
    柏拉图：通过思考和探究，通过不断地提问和回答，去探索真理和智慧。
    苏格拉底：是的，我们通过对话的方式，引导人们思考，帮助他们发现自己内心的智慧和真理。这就是我著名的“苏格拉底式对话”。



```python
with open('conversation.txt', 'w', encoding='utf-8') as f:
    for msg in all_msg_history:
        f.write(f"{msg.get('name')}：{msg.get('content')}\n")
```


```python

```
