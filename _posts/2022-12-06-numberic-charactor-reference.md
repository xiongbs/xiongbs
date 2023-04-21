---
title: 文本highlight 和 numberic character reference 解析emoji和HTML Entities 实体符号
author: beni
date: Tue Dec 06 2022 14:36:01 GMT+0800
categories: [diary]
tags: [NCR, unicode, HTML Entities]
mermaid: true
---


# 文本highlight 和 numberic character reference 解析emoji和HTML 

    需求：需要搜索结果高亮文本，查看NG-ZORRO ng-zorro-antd源码如何实现，源码如下，发现了一段有趣的正则处理

```typescript
/**
 * Use of this source code is governed by an MIT-style license that can be
 * found in the LICENSE file at https://github.com/NG-ZORRO/ng-zorro-antd/blob/master/LICENSE
 */

import { Pipe, PipeTransform } from '@angular/core';

// Regular Expressions for parsing tags and attributes
// 正对emoji字符做处理，转换unicode emoji 转换为html NCR数字字符引用 十进制表示
const SURROGATE_PAIR_REGEXP = /[\uD800-\uDBFF][\uDC00-\uDFFF]/g;
// ! to ~ is the ASCII range.
const NON_ALPHANUMERIC_REGEXP = /([^\#-~ |!])/g;

/**
 * Escapes all potentially dangerous characters, so that the
 * resulting string can be safely inserted into attribute or
 * element text.
 */
function encodeEntities(value: string): string {
  return value
    .replace(/&/g, '&amp;')
    .replace(SURROGATE_PAIR_REGEXP, (match: string) => {
      const hi = match.charCodeAt(0);
      const low = match.charCodeAt(1);
      return `&#${(hi - 0xd800) * 0x400 + (low - 0xdc00) + 0x10000};`; // 将所有emoji转换为十进制的html字符再展示
    })
    .replace(NON_ALPHANUMERIC_REGEXP, (match: string) => `&#${match.charCodeAt(0)};`) // 转换为十进制的html字符
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;');
}

@Pipe({
  name: 'nzHighlight',
  pure: true
})
export class NzHighlightPipe implements PipeTransform {
  private UNIQUE_WRAPPERS: [string, string] = ['##==-open_tag-==##', '##==-close_tag-==##'];

  transform(value: string, highlightValue: string, flags?: string, klass?: string): string | null {
    if (!highlightValue) {
      return value;
    }

    // Escapes regex keyword to interpret these characters literally
    // 需要转义的字符加上\\符号，防止被执行转义 比如保证构造正则不会出错
    const searchValue = new RegExp(highlightValue.replace(/([.*+?^=!:${}()|[\]\/\\])/g, '\\$&'), flags);
    const wrapValue = value.replace(searchValue, `${this.UNIQUE_WRAPPERS[0]}$&${this.UNIQUE_WRAPPERS[1]}`);
    return encodeEntities(wrapValue)
      .replace(new RegExp(this.UNIQUE_WRAPPERS[0], 'g'), klass ? `<span class="${klass}">` : '<span>')
      .replace(new RegExp(this.UNIQUE_WRAPPERS[1], 'g'), '</span>');
  }
}

```

主要在 **SURROGATE_PAIR_REGEXP** 这个正则匹配，函数先将几个特定特定的字符转义为html Entities的字符，防止被匹配到。参考[html Character references](https://www.w3.org/TR/2017/WD-html52-20170228/syntax.html#character-references);
emoji转换的示例

```javascript 
// \ud83d\ude29 比如当前的代码emoji😩

```