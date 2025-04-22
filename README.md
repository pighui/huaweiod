# huaweiod
![题1](https://github.com/user-attachments/assets/9b39b165-e3ab-45c5-a2ee-c4a82b52a273)
### 方法思路

1. **单词内部调整**：将每个单词的字母按字典序重新排序。这里字典序是指按照字符的ASCII码值进行排序。
2. **单词间调整**：
   1. **统计频率**：统计每个处理后的单词出现的次数。
   2. **排序规则**：首先按出现次数降序排列；次数相同则按单词长度升序排列；次数和长度都相同则按字典序升序排列。

### 解决代码

```python
from collections import Counter

s = input().split()
processed = [''.join(sorted(word)) for word in s]
count = Counter(processed)
sorted_words = sorted(processed, key=lambda x: (-count[x], len(x), x))
print(' '.join(sorted_words))
```

### 代码解释

1. **输入处理**：读取输入字符串并将其按空格分割成单词列表。
2. **单词内部排序**：对每个单词的字符进行排序，生成新的单词列表。
3. **统计频率**：使用`Counter`统计处理后的每个单词的出现次数。
4. **排序规则**：根据频率降序、长度升序、字典序升序的规则对处理后的单词进行排序。
5. **输出结果**：将排序后的单词列表用空格连接并输出。

该方法确保了每个步骤都符合题目要求，并且通过合理使用Python内置函数和数据结构，保证了代码的高效性和简洁性。

![题2](https://github.com/user-attachments/assets/7e228f7a-cdf5-4c34-a837-425c9e3972e3)

为了解决这个问题，我们需要根据指示灯的坐标信息对它们进行排序。排序规则要求先按行排序，行内再按列排序。具体步骤如下：

### 方法思路

1. **计算中心点**：每个指示灯的位置由其左上角坐标 (x1, y1) 和右下角坐标 (x2, y2) 确定。中心点坐标 (x_center, y_center) 可以通过这两个点计算得到。
2. **行排序**：首先按行的垂直位置（中心点的 y 坐标）升序排列，确保较高的行优先处理。
3. **列排序**：在同一行内，按列的水平位置（中心点的 x 坐标）升序排列，确保从左到右处理。

### 解决代码

```python
n = int(input())
lamps = []

for _ in range(n):
    parts = input().split()
    lamp_id = parts[0]
    x1, y1, x2, y2 = map(int, parts[1:5])
    x_center = (x1 + x2) / 2
    y_center = (y1 + y2) / 2
    lamps.append((y_center, x_center, lamp_id))

# 按y_center升序，x_center升序排序
lamps.sort(key=lambda x: (x[0], x[1]))

result = [lamp[2] for lamp in lamps]
print(' '.join(result))
```

### 代码解释

1. **读取输入**：首先读取指示灯的数量 `n`，然后逐行读取每个灯的编号及其坐标信息。
2. **计算中心点**：通过坐标点计算每个灯的中心点坐标。
3. **排序处理**：根据中心点的 y 坐标升序排列行，同一行内按 x 坐标升序排列列。
4. **输出结果**：将排序后的灯编号按顺序拼接成字符串输出。

该方法确保了按题目要求的行和列的顺序进行排序，处理高效且逻辑清晰。

![题3-1](https://github.com/user-attachments/assets/b639aaf7-15cb-41df-9ad1-5d18c857797f)

为了解决这个问题，我们需要将通信信道分配给尽可能多的用户，使得每个用户的总信道容量满足其需求。我们采用贪心算法，优先使用容量较大的信道，以尽可能减少每个用户所需的信道数量，从而最大化用户数量。

### 方法思路

1. **输入处理**：读取最大阶数、各阶信道的数量以及用户需求。
2. **按阶排序**：将信道按阶从高到低排序，以便优先使用容量较大的信道。
3. **贪心分配**：对于每个用户，从最高阶开始依次尝试使用信道，直到满足需求。每次尽可能多地使用当前阶的信道，若不足则继续使用下一阶的信道，直到满足需求或遍历所有阶。
4. **更新资源**：每次成功分配后，更新剩余的信道数量。

### 解决代码

```python
R = int(input())
counts = list(map(int, input().split()))
D = int(input())

# 阶按从高到低排序，例如R=5时，顺序是5,4,3,2,1,0
sorted_orders = list(range(R, -1, -1))

result = 0

while True:
    sum_val = 0
    used = {}
    for r in sorted_orders:
        cap = 2 ** r
        remaining = D - sum_val
        if remaining <= 0:
            break
        if cap == 0:
            continue
        need = (remaining + cap - 1) // cap  # 向上取整
        available = counts[r]
        take = min(available, need)
        if take > 0:
            used[r] = take
            sum_val += take * cap
    if sum_val >= D:
        result += 1
        # 扣除使用的数量
        for r in used:
            counts[r] -= used[r]
    else:
        break

print(result)
```

### 代码解释

1. **输入处理**：读取输入的最大阶数 `R`、各阶信道的数量 `counts` 和用户需求 `D`。
2. **阶排序**：将阶从高到低排序，以便优先使用大容量信道。
3. **贪心分配**：对每个用户，从最高阶开始计算所需信道数量，直到满足需求。使用字典 `used` 记录各阶使用的信道数量。
4. **更新资源**：若当前用户满足需求，扣除已使用的信道数量，继续处理下一个用户；否则终止循环。

该方法确保了尽可能高效地使用信道资源，从而最大化可满足的用户数量。通过贪心策略优先使用大容量信道，减少了每个用户所需的信道数量，提高了资源利用率。
