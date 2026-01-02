# Badge

图标右上角的圆形徽标数字。

## 何时使用

- 一般出现在通知图标或头像的右上角，用于显示需要处理的消息条数，通过醒目视觉形式吸引用户处理。
- 在有新消息、讯息时，或者是 app/插件/功能模块可以更新、升级时使用这个组件。

## 基本

```tsx
/**
 * title: 基本
 * description: 简单的徽章展示，当 `count` 为 `0` 时，默认不显示，但是可以使用 `showZero` 修改为显示。
 */

import * as React from 'react';

import { Badge } from '@teamix/ui';
import './basic.css';

export default () => (
  <div>
    <Badge count={5}>
      <a href="#" className="basic-example"></a>
    </Badge>
    <Badge count={0} showZero>
      <a href="#" className="basic-example"></a>
    </Badge>
  </div>
);
```

```css
.basic-example {
    display: inline-block;
    width: 42px;
    height: 42px;
    border-radius: 8px;
    background: #eee;
}

.next-badge {
    margin-right: 16px;
}
```

## 封顶数字

```tsx
/**
 * title: 封顶数字
 * description: 过 `overflowCount`的数值，会显示`${overflowCount}+`，`overflowCount` 为`99`。
 */

import * as React from 'react';

import { Badge } from '@teamix/ui';
import './overflowCount.css';

export default () => (
  <div>
    <Badge count={99}>
      <a href="#" className="head-example"></a>
    </Badge>
    <Badge count={100}>
      <a href="#" className="head-example"></a>
    </Badge>
    <Badge count={100} overflowCount={10}>
      <a href="#" className="head-example"></a>
    </Badge>
    <Badge count={1000} overflowCount={999}>
      <a href="#" className="head-example"></a>
    </Badge>
  </div>
);
```

```css
.next-badge {
    margin-right: 16px;
}
.head-example {
    display: inline-block;
    width: 42px;
    height: 42px;
    border-radius: 8px;
    background: #eee;
}
```

## 讨嫌的小红点

```tsx
/**
 * title: 讨嫌的小红点
 * description: 没有具体的数字，仅展示小红点。
 */

import * as React from 'react';

import { Badge, Icon } from '@teamix/ui';
import './dot.css';

export default () => (
  <div>
    <Badge dot>
      <Icon type="email" />
    </Badge>
    <Badge count={0} dot>
      <Icon type="email" />
    </Badge>
    <Badge dot>
      <a href="#">A Link</a>
    </Badge>
  </div>
);
```

```css
.next-badge {
    margin-right: 16px;
}
```

## 自定义图标、颜色等

```tsx
/**
 * title: 自定义图标、颜色等
 * description: 通过 `content` 属性可以自定义徽标的内容，自定义内容不包含任何色彩样式，完全由使用者自己定义。
 */

import * as React from 'react';

import { Badge, Icon } from '@teamix/ui';
import './content.css';

export default () => (
  <div>
    <Badge
      content="hot"
      style={{ backgroundColor: '#FC0E3D', color: '#FFFFFF' }}
    >
      <a href="#" className="head-example"></a>
    </Badge>
    <Badge
      content={<Icon type="error" />}
      style={{ backgroundColor: 'transparent', color: 'red', padding: 0 }}
    >
      <a href="#" className="head-example"></a>
    </Badge>
  </div>
);
```

```css
.next-badge {
    margin-right: 24px;
}
.head-example {
    display: inline-block;
    width: 42px;
    height: 42px;
    border-radius: 8px;
    background-color: #eee;
}
```
## 独立使用

```tsx
/**
 * title: 独立使用
 * description: 不包裹任何元素即独立使用，可自定样式展示。
 */

import * as React from 'react';

import { Badge } from '@teamix/ui';
import './no-wrapper.css';

export default () => (
  <div>
    <Badge count={25} />
    <Badge
      count={4}
      style={{
        backgroundColor: '#fff',
        color: '#999',
        border: '1px solid #d9d9d9',
      }}
    />
    <Badge count={109} style={{ backgroundColor: '#87d068' }} />
    <Badge dot />
    <Badge
      content="hot"
      style={{ backgroundColor: '#FC0E3D', color: '#FFFFFF' }}
    />
  </div>
);
```

```css
.next-badge {
    margin-right: 16px;
}
```

## 徽标可点击

```tsx
/**
 * title: 徽标可点击
 * description: 用 `<a>` 件，实现徽标本身可点击。
 */

import * as React from 'react';

import { Badge } from '@teamix/ui';
import './clickable.css';

export default () => (
  <a href="#">
    <Badge count={5}>
      <span className="basic-example"></span>
    </Badge>
  </a>
);
```

```css
.basic-example {
    display: inline-block;
    width: 42px;
    height: 42px;
    border-radius: 8px;
    background: #eee;
}

.next-badge {
    margin-right: 16px;
}
```

## 内容动态变化

```tsx
/**
 * title: 内容动态变化
 * description: 展示徽标内容动态变化的效果。
 */
import React, { useState } from 'react';
import { Badge, Button, Icon } from '@teamix/ui';
import './dynamic-content.css';

const ButtonGroup = Button.Group;

const Demo = () => {
  const [count, setCount] = useState(0);
  const [show, setShow] = useState(true);

  const increase = () => {
    setCount(count + 1);
  };

  const decrease = () => {
    const newCount = count - 1;
    setCount(newCount < 0 ? 0 : newCount);
  };

  const onClick = () => {
    setShow(!show);
  };

  return (
    <div>
      <div className="change-count">
        <Badge count={count}>
          <a href="#" className="head-example">
            <span className="next-sr-only">unread messages</span>
          </a>
        </Badge>
        <Badge count={count} showZero>
          <a href="#" className="head-example">
            <span className="next-sr-only">unread messages</span>
          </a>
        </Badge>
        <ButtonGroup>
          <Button aria-label="add" onClick={increase}>
            <Icon type="add" />
          </Button>
          <Button aria-label="minus" onClick={decrease}>
            <Icon type="minus" />
          </Button>
        </ButtonGroup>
      </div>
      <div>
        <Badge dot={show}>
          <a href="#" className="head-example"></a>
        </Badge>
        <Button onClick={onClick}>Toggle Display</Button>
      </div>
    </div>
  );
};

export default Demo;
```

```css
.next-badge {
    margin-right: 16px;
}
.change-count {
    margin-bottom: 16px;
}
.head-example {
    display: inline-block;
    width: 42px;
    height: 42px;
    border-radius: 8px;
    background: #eee;
}
```

## 无障碍支持

```tsx
/**
 * title: 无障碍支持
 * description: 可通过给内容添加`className="next-sr-only"`，使内容仅能被读屏软件读取，但不会展示到页面上。
 */

import * as React from 'react';

import { Badge } from '@teamix/ui';
import './accessibility.css';

export default () => (
  <div>
    <Badge count={5}>
      <a href="#" className="basic-example">
        <span className="next-sr-only">unread messages</span>
      </a>
    </Badge>
  </div>
);
```

```css
.basic-example {
    display: inline-block;
    width: 42px;
    height: 42px;
    border-radius: 8px;
    background: #eee;
}

.next-badge {
    margin-right: 16px;
}
```

## API

### Badge

| 参数          | 说明                                                                             | 类型          | 默认值 | 版本支持 |
| ------------- | -------------------------------------------------------------------------------- | ------------- | ------ | -------- |
| children      | 徽标依托的内容，一般显示在其右上方                                               | ReactNode     | -      |          |
| count         | 展示的数字，大于 `overflowCount` 时显示为 `${overflowCount}+`，为 `0` 时默认隐藏 | Number/String | 0      |          |
| showZero      | 当 `count` 为 `0` 时，是否显示 count                                             | Boolean       | false  | 1.16     |
| content       | 自定义徽标中的内容                                                               | ReactNode     | -      |          |
| overflowCount | 展示的封顶的数字                                                                 | Number/String | 99     |          |
| dot           | 不展示数字，只展示一个小红点                                                     | Boolean       | false  |          |

## 无障碍键盘操作指南

- `Badge` 组件本身不涉及键盘操作，是 `Badge`与其`chilren` 击；
- 无障碍配置方式参见[#无障碍支持](#accessibility-container)。

## 说明
来自于https://gitee.com/tongyilingma/ui-components-wiki/blob/master/basic/badge.md