---
title: Chemical Formula(Bytedance)
author: beni
date: Fri Aug 19 2022 17:29:17 GMT+0800
categories: [Blogging, FrontEnd, Alogrithem]
tags: [JS, Bytedance, recursion]
mermaid: true
---


给定一个用字符串展示的化学公式，计算每种元素的个数。
规则如下：
* 元素命名采用驼峰命名，例如 Mg
* () 代表内部的基团，代表阴离子团
* [] 代表模内部链节通过化学键的连接，并聚合
例如：H2O => H2O1 Mg(OH)2 => H2Mg1O2

## 格式

> 输入：
> - 化学公式的字符串表达式，例如：K4[ON(SO3)2]2 。
> - 输出：
> 元素名称及个数：K4N2O14S4，并且按照元素名称升序排列。

## 示例
> 输入： K4[ON(SO3)2]2   
> 输出： K4N2O14S4


## 我的代码

```javascript
function parseChemical(str) {
  if (!str) {
    return str;
  }

  const ans = generateSubstr(str, 1);
  if (!ans) {
    return ans;
  }
  let i = 0;
  let num = 0;
  let val = '';
  const map = new Map();
  /*
  map: {
    元素key：元素的数量
  }
  */
  while (i < ans.length) {
    let ch = ans[i];
    let next = ans[i + 1];
    if (isChar(ch)) {
      val += ch;
      if (isUpper(next)) {
        if (map.has(val)) {
          map.set(val, map.get(val) + 1);
        } else {
          map.set(val, 1);
        }
        val = '';
      }
    } else {
      num = num * 10 + parseInt(ch);
      if (isChar(next) || !next) {
        if (map.has(val)) {
          map.set(val, map.get(val) + num);
        } else {
          map.set(val, num);
        }
        val = '';
        num = 0;
      }
    }

    i++;
  }
  let entires = [...map.entries()];
  // 按照首字母大小写 排序
  entires.sort((a, b) => {
    const [key1] = a;
    const [key2] = b;
    return key1.charCodeAt(0) - key2.charCodeAt(0);
  });
  // 凭借字符串
  return entires.reduce((prev, current) => {
    const [key, value] = current;
    return prev + key + value;
  }, '');
}

function generateSubstr(str, multiple) {
  // multiple 倍数
  if (!str) {
    return str;
  }

  let ans = '';
  const len = str.length;
  let i = 0;
  let count = 0;
  let substr = '';
  while (i < len) {
    let ch = str[i];
    if (ch === '(' || ch === '[') {
      // 记录括号数
      count++;
      // 下一个循环
      i++;
      // 如果子串不为空，说明是子串中的括号符，需要保留
      if (substr !== '') {
        substr += ch;
      } else {
        // 如果是第一个括号，直接忽略掉
        continue;
      }
    } else if (ch === ')' || ch === ']') {
      count--;
      if (count === 0) {
        // 如果count为0表示递归完
        // 求子串, 子串符号，和倍数
        i++;
        ans += generateSubstr(substr, str[i]);
        substr = '';
      } else {
        substr += ch;
      }

      i++;
    } else {
      // 当前括号为空
      if (count === 0) {
        ans += ch;
      } else {
        // 括号不为空，就放在substr里
        substr += ch;
      }
      i++;
    }
  }

  if (multiple <= 1) {
    return ans;
  }
  let newAns = '';

  let j = 0;
  while (j < ans.length) {
    let ch2 = ans[j];
    j++;
    let next = ans[j];
    if (ch2 > '9' && next <= '9') {
      ch2 += next * multiple;

      j++;
    } else {
      ch2 += multiple;
    }

    newAns += ch2;
  }

  return newAns;
}

function isChar(ch) {
  return islowwer(ch) || isUpper(ch);
}

function isUpper(ch) {
  return ch >= 'A' && ch <= 'Z';
}

function islowwer(ch) {
  return ch >= 'a' && ch <= 'z';
}

function isDigit(ch) {
  return ch >= '1' && ch <= '9';
}

console.log(parseChemical('K4[ON(SO3)2]2'));

```


## 值得参考的算法方案

```javascript

```

## 总结
递归调用  
注意判断细节  
最后的排序
