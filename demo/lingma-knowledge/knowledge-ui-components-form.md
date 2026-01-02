# Form

表单组件。

## 何时使用

表单布局、校验、数据提交操作时用到。 组件的设计思想可以看这篇文章 <a href="https://zhuanlan.zhihu.com/p/56280821" target="_blank">https&#x3A;//zhuanlan.zhihu.com/p/56280821</a>

## 如何使用

- 组件不要使用关键字 `nodeName` 作为 name、id
- Form 默认使用 `size=medium`, 并且会控制 FormItem 内所有组件的 size。 如果想修改组件的 size `<FormItem size="small" >`
- 在垂直表单中如果文字（一般 `<p>` 标签）或者组件向上偏离，可以通过 `className="next-form-text-align"` 辅助调整
- 必须是被 `<FormItem>`直接包裹的组件才能展示校验错误提示。如果界面不展示错误信息，请检查是否有多个层级。 比如 `<FormItem><div><Input/></div></FormItem>` 是无法展示校验信息的。
- 可以通过 `<Form field={false}>` 来关闭数据获取，变成一个纯布局组件

### 关于校验

- 建议一个 FormItem 放一个组件, 方便错误提示跟随组件展示
- 组件必须是 FormItem 的第一层子元素
- 详细校验请查看 `Field` 组件文档的 rules

### 复杂表单场景

如果您的表单场景非常复杂，比如动态渲染，大量字段，复杂数据结构，复杂联动校验，可以考虑使用 [formily](https://github.com/alibaba/formily)，formily 已经封装了所有 fusion 组件，保证您开箱即用
## 基本
```tsx
/**
 * title: 基本
 * description: 表单布局、编辑、提交、校验的基本使用
 */
import React from 'react';
import { Form, Input, Checkbox } from '@teamix/ui';
const FormItem = Form.Item;

const formItemLayout = {
  labelCol: {
    fixedSpan: 8,
  },
  wrapperCol: {
    span: 14,
  },
};

const Demo = () => {

  const handleSubmit = (values) => {
    console.log('value & errors', values);
  };

  return (
    <Form style={{ width: '60%' }} {...formItemLayout} colon>
    <FormItem
      label="Username:"
      required
      requiredMessage="Please input your username!"
    >
      <Input name="baseUser" />
    </FormItem>
    <FormItem
      label="Password:"
      required
      requiredMessage="Please input your password!"
    >
      <Input.Password name="basePass" placeholder="Please Enter Password" />
    </FormItem>
    <FormItem label=" " colon={false}>
      <Checkbox name="agreement" defaultChecked>
        Agree
      </Checkbox>
    </FormItem>
    <FormItem label=" " colon={false}>
      <Form.Submit
        type="primary"
        validate
        onClick={handleSubmit}
        style={{ marginRight: 8 }}
      >
        Submit
      </Form.Submit>
      <Form.Reset>Reset</Form.Reset>
    </FormItem>
  </Form>
  );
};

export default Demo;

```
## 布局
```tsx
/**
 * title: 布局
 * description: inline 布局只支持横排
 */

import * as React from 'react';

import { Form, Field, Input, Radio, Switch } from '@teamix/ui';

function handleSubmit(v) {
  console.log(v);
}

const formItemLayout = {
  labelCol: { span: 4 },
  wrapperCol: { span: 14 },
};

const App = () => {
  const field = Field.useField({
    autoUnmount: false,
    values: { inline: false, labelAlign: 'left' },
  });
  const inline = field.getValue('inline');
  const labelAlign = inline ? 'left' : field.getValue('labelAlign');
  const layout = inline ? {} : formItemLayout;

  return (
    <Form field={field} inline={inline} labelAlign={labelAlign} {...layout}>
      <Form.Item label="Inline Layout">
        <Switch name="inline" />
      </Form.Item>

      {inline ? null : (
        <Form.Item label="Label align">
          <Radio.Group shape="button" name="labelAlign">
            <Radio value="left">left</Radio>
            <Radio value="top">top</Radio>
            <Radio value="inset">inset</Radio>
          </Radio.Group>
        </Form.Item>
      )}

      <Form.Item label="Username:">
        <Input name="inlineUser" placeholder="first" />
      </Form.Item>
      <Form.Item label="Password:" hasFeedback={false}>
        <Input.Password
          name="inlinePass"
          placeholder="Please enter your password!"
        />
      </Form.Item>

      <Form.Item label=" ">
        <Form.Submit onClick={handleSubmit}>Submit</Form.Submit>
      </Form.Item>
    </Form>
  );
};

export default () => <App />;
```
## 尺寸
```tsx
/**
 * title: 尺寸
 * description: <code>size</code> 会强制设置 `FormItem` 下的所有组件的size
 */
import React, { useState } from 'react';
import {
  Form,
  Input,
  Select,
  Radio,
  NumberPicker,
  DatePicker,
  Switch,
  Button,
} from '@teamix/ui';
import './size.css';

const FormItem = Form.Item;
const Option = Select.Option;
const formItemLayout = {
  labelCol: { span: 8 },
  wrapperCol: { span: 16 },
};

const Demo = () => {
  const [size, setSize] = useState('medium');

  const handleChange = (v) => {
    setSize(v);
  };

  return (
    <div>
      <Form
        {...formItemLayout}
        size={size}
        style={{ maxWidth: '500px' }}
      >
        <FormItem label="Size:">
          <Radio.Group
            shape="button"
            value={size}
            onChange={handleChange}
          >
            <Radio value="small">small</Radio>
            <Radio value="medium">medium</Radio>
            <Radio value="large">large</Radio>
          </Radio.Group>
        </FormItem>
        <FormItem label="Input:">
          <Input placeholder="Please enter your user name" id="userName" />
        </FormItem>
        <FormItem label="Select:">
          <Select>
            <Option value="test">test</Option>
          </Select>
        </FormItem>
        <FormItem label="NumberPicker:">
          <NumberPicker />
        </FormItem>
        <FormItem label="DatePicker:">
          <DatePicker />
        </FormItem>
        <FormItem label="Switch:">
          <Switch />
        </FormItem>
      </Form>
    </div>
  );
};

export default Demo;
```
```css
.demo-ctl {
  background-color: #f1f1f1;
  padding: 10.0px;
  color: #0a7ac3;
  border-left: 4.0px solid #0d599a;
}
```
## 注册
```tsx
/**
 * title: 注册
 * description: 验证码获取
 */
import React, { useState, useEffect } from 'react';
import { Form, Input } from '@teamix/ui';

const FormItem = Form.Item;

const formItemLayout = {
  labelCol: { fixedSpan: 3 },
  wrapperCol: { span: 20 },
};

const Demo = () => {
  const [code, setCode] = useState('');
  const [second, setSecond] = useState(60);

  const handleSubmit = (values) => {
    console.log('Get form value:', values);
  };

  const sendCode = () => {
    setCode(Math.floor(Math.random() * (999999 - 99999 + 1) + 99999));
    startCountdown();
  };

  const startCountdown = () => {
    const intervalId = setInterval(() => {
      setSecond(second - 1);
      if (second === 0) {
        clearInterval(intervalId);
        setCode('');
        setSecond(60);
      }
    }, 1000);
  };

  useEffect(() => {
    if (code) {
      startCountdown();
    }
    // 清理函数，清除计时器
    return () => clearInterval(startCountdown);
  }, [code]);

  return (
    <Form
      style={{ width: 400 }}
      {...formItemLayout}
      labelTextAlign="left"
      size="large"
      labelAlign="inset"
    >
      <FormItem label="name" required asterisk={false}>
        <Input name="username" trim defaultValue="frank" />
      </FormItem>
      <FormItem label="phone" format="tel" required asterisk={false}>
        <Input
          name="phone"
          trim
          innerAfter={
            <Form.Submit
              text
              type="primary"
              disabled={!!code}
              validate={['phone']}
              onClick={sendCode}
              style={{ marginRight: 10 }}
            >
              {code ? `retry after ${second}s` : 'send code'}
            </Form.Submit>
          }
        />
      </FormItem>
      {code ? (
        <FormItem label="code" required asterisk={false}>
          <Input name="code" trim defaultValue={code} />
        </FormItem>
      ) : null}

      <FormItem label=" ">
        <Form.Submit
          style={{ width: '100%' }}
          type="primary"
          validate
          onClick={handleSubmit}
        >
          Submit
        </Form.Submit>
      </FormItem>
    </Form>
  );
};

export default () => <Demo />;
```
## 嵌套
```tsx
/**
 * title: 嵌套
 * description: FormItem 嵌套
 */

import * as React from 'react';

import { Form, Input, Grid } from '@teamix/ui';

const FormItem = Form.Item;
const { Row, Col } = Grid;

const formItemLayout = {
  labelCol: { span: 4 },
  wrapperCol: { span: 14 },
};

const insetLayout = {
  labelCol: { fixedSpan: 3 },
};

export default () => (
  <Form {...formItemLayout}>
    <FormItem id="control-input" label="Input Something：">
      <Row gutter="4">
        <Col>
          <FormItem
            style={{ margin: 0 }}
            label="Nest"
            labelAlign="inset"
            {...insetLayout}
            required
            requiredTrigger="onBlur"
            asterisk={false}
          >
            <Input placeholder="Please enter..." name="firstname" />
          </FormItem>
        </Col>
        <Col>
          <FormItem
            style={{ margin: 0 }}
            label="Nest"
            labelAlign="inset"
            {...insetLayout}
            required
            asterisk={false}
          >
            <Input placeholder="need onChange" name="secondname" />
          </FormItem>
        </Col>
      </Row>
    </FormItem>
    <FormItem label="Bank Account：">
      <Row gutter="4">
        <Col>
          <FormItem required requiredTrigger="onBlur">
            <Input name="A" />
          </FormItem>
        </Col>
        <Col>
          <FormItem required requiredTrigger="onBlur">
            <Input name="B" />
          </FormItem>
        </Col>
        <Col>
          <FormItem required requiredTrigger="onBlur">
            <Input name="C" />
          </FormItem>
        </Col>
        <Col>
          <FormItem required requiredTrigger="onBlur">
            <Input name="D" />
          </FormItem>
        </Col>
      </Row>
    </FormItem>
    <FormItem label=" ">
      <Form.Submit validate onClick={v => console.log(v)}>
        Submit
      </Form.Submit>
    </FormItem>
  </Form>
);
```
## 自定义布局
```tsx
/**
 * title: 自定义布局
 * description: 标签位置：上、左
 */
import React, { useState } from 'react';
import { Form, Input, Switch, Grid, Button, Icon, Balloon } from '@teamix/ui';

const FormItem = Form.Item;
const { Row, Col } = Grid;

const style = {
  padding: '20px',
  background: '#F7F8FA',
  margin: '20px',
};
const formItemLayout = {
  labelCol: { fixedSpan: 4 },
};
const label = (
  <span>
    name：
    <Balloon
      type="primary"
      trigger={<Icon type="help" size="small" />}
      closable={false}
    >
      blablablablablablablabla
    </Balloon>
  </span>
);

const Demo = () => {
  const [labelAlign, setLabelAlign] = useState('top');

  const handleChange = (v) => {
    setLabelAlign(v ? 'left' : 'top');
  };

  return (
    <div>
      <Form inline>
        <Form.Item label="label Position">
          <Switch
            checked={labelAlign === 'left'}
            onChange={handleChange}
          />
        </Form.Item>
      </Form>

      <Form style={style}>
        <Row gutter="4">
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label={label}
              required
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Long search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Long search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Long search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
        </Row>
        <Row>
          <Col style={{ textAlign: 'right' }}>
            <Button type="primary" style={{ marginRight: '5px' }}>
              Search
            </Button>
            <Button>Clear All</Button>
          </Col>
        </Row>
      </Form>

      <Form style={style}>
        <Row gutter="4">
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label={label}
              required
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Long search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
          <Col>
            <FormItem
              {...formItemLayout}
              labelAlign={labelAlign}
              label="Search name:"
            >
              <Input placeholder="Enter a search name:" />
            </FormItem>
          </Col>
        </Row>
        <Row>
          <Col style={{ textAlign: 'right' }}>
            <Button type="primary" style={{ marginRight: '5px' }}>
              Search
            </Button>
            <Button>Clear All</Button>
          </Col>
        </Row>
      </Form>
    </div>
  );
};

export default Demo;
```
## 自适应布局
```tsx
/**
 * title: 自适应布局
 * description:
 */
import React, { useState } from 'react';
import {
  Form,
  Input,
  Switch,
  Grid,
  Button,
  Icon,
  Balloon,
  ConfigProvider,
} from '@teamix/ui';

const FormItem = Form.Item;

const style = {
  padding: '20px',
  background: '#F7F8FA',
  margin: '20px',
};
const formItemLayout = {
  labelWidth: 100,
  colSpan: 4,
};

const label = (
  <span>
    name：
    <Balloon
      type="primary"
      trigger={<Icon type="help" size="small" />}
      closable={false}
    >
      blablablablablablablabla
    </Balloon>
  </span>
);

const Demo = () => {
  const [labelAlign, setLabelAlign] = useState('top');
  const [device, setDevice] = useState('desktop');

  const handleChange = (v) => {
    setLabelAlign(v ? 'left' : 'top');
  };

  const handleDeviceChange = (newDevice) => {
    setDevice(newDevice);
  };

  return (
    <ConfigProvider device={device}>
      <div>
        <h3>Label Position</h3>
        <Switch
          // checkedChildren="left"
          // unCheckedChildren="top"
          checked={labelAlign === 'left'}
          onChange={handleChange}
        />
        <Button onClick={() => handleDeviceChange('desktop')}>desktop</Button>
        <Button onClick={() => handleDeviceChange('tablet')}>tablet</Button>
        <Button onClick={() => handleDeviceChange('phone')}>phone</Button>

        <Form style={style} responsive>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label={label}
            required
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Long search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Long search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Long search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem
            {...formItemLayout}
            labelAlign={labelAlign}
            label="Search name:"
          >
            <Input placeholder="Enter a search name:" />
          </FormItem>
          <FormItem colSpan={12} style={{ textAlign: 'right' }}>
            <Button type="primary" style={{ marginRight: '5px' }}>
              Search
            </Button>
            <Button>Clear All</Button>
          </FormItem>
        </Form>
      </div>
    </ConfigProvider>
  );
};

export default Demo;
```
## 回车提交
```tsx
/**
 * title: 回车提交
 * description: 需要Form里面有 htmlType="submit" 的元素
 */
import React from 'react';
import { Form, Input } from '@teamix/ui';

const FormItem = Form.Item;

const Demo = () => {
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('onsubmit');
  };

  return (
    <Form onSubmit={handleSubmit}>
      <FormItem>
        <Input placeholder="Enter Key can also trigger ‘onSubmit’" />
      </FormItem>
      <Form.Submit htmlType="submit">submit</Form.Submit>
    </Form>
  );
};

export default Demo;
```

## 响应式
```tsx
/**
 * title: 响应式
 * description: 可以通过配置 `labelCol` `wrapperCol` 的 `Grid.Col` 响应式属性实现响应式
 */

import * as React from 'react';

import { Form, Input, Select } from '@teamix/ui';

const FormItem = Form.Item;

const formItemLayout = {
  labelCol: { xxs: 4, l: 4 },
  wrapperCol: { xxs: 20, l: 16 },
};

export default () => (
  <Form {...formItemLayout}>
    <FormItem label="userName:">
      <Input />
    </FormItem>
    <FormItem label="password:">
      <Input
        htmlType="password"
        name="resPass"
        placeholder="Please Enter Password"
      />
    </FormItem>
    <FormItem label="Country:">
      <Select placeholder="Please select a country" style={{ width: '100%' }}>
        <option value="china">China</option>
        <option value="use">United States</option>
        <option value="japan">Japan</option>
        <option value="korean">South Korea</option>
        <option value="Thailand">Thailand</option>
      </Select>
    </FormItem>
    <FormItem label="Note:" help="something">
      <Input.TextArea placeholder="something" name="resReremark" />
    </FormItem>
    <FormItem label=" ">
      <Form.Submit>Submit</Form.Submit>
    </FormItem>
  </Form>
);
```
## 校验提示
```tsx
/**
 * title: 校验
 * description: 基本的表单校验例子。
 */
import React from 'react';
import { Form, Input, Radio } from '@teamix/ui';

const FormItem = Form.Item;
const RadioGroup = Radio.Group;

const formItemLayout = {
  labelCol: {
    span: 6,
  },
  wrapperCol: {
    span: 14,
  },
};

const BasicDemo = () => {

  const userExists = (rule, value) => {
    return new Promise((resolve, reject) => {
      if (!value) {
        resolve();
      } else {
        setTimeout(() => {
          if (value === 'frank') {
            reject([new Error('Sorry, this username is already exist.')]);
          } else {
            resolve();
          }
        }, 500);
      }
    });
  }

  return (
    <Form {...formItemLayout}>
        <FormItem
          label="Account:"
          hasFeedback
          validator={userExists}
          help=""
        >
          <Input placeholder="Input frank" name="valUsername" />
          <Form.Error name="valUsername">
            {(errors, state) => {
              if (state === 'loading') {
                return 'loading...';
              } else {
                return errors;
              }
            }}
          </Form.Error>
        </FormItem>
        <FormItem
          label="Email:"
          hasFeedback
          required
          requiredTrigger="onBlur"
          format="email"
        >
          <Input
            placeholder="Both trigget onBlur and onChange"
            name="valEmail"
          />
        </FormItem>

        <FormItem
          label="Password:"
          hasFeedback
          required
          requiredMessage="Please enter password"
        >
          <Input htmlType="password" name="valPasswd" />
        </FormItem>

        <FormItem
          label="Gender:"
          hasFeedback
          required
          requiredMessage="Please select your gender"
        >
          <RadioGroup name="valSex">
            <Radio value="male">Male</Radio>
            <Radio value="female">Female</Radio>
          </RadioGroup>
        </FormItem>

        <FormItem
          label="Remarks:"
          required
          requiredMessage="Really do not intend to write anything?"
        >
          <Input.TextArea
            maxLength={20}
            showLimitHint
            placeholder="Everything is ok!"
            name="valTextarea"
          />
        </FormItem>

        <FormItem wrapperCol={{ offset: 6 }}>
          <Form.Submit
            validate
            type="primary"
            onClick={(v, e) => console.log(v, e)}
            style={{ marginRight: 10 }}
          >
            Submit
          </Form.Submit>
          <Form.Reset>Reset</Form.Reset>
        </FormItem>
      </Form>
  );
};

export default BasicDemo;
```
## 校验
```tsx
/**
 * title: 校验
 * description: 基本的表单校验例子。
 */
import React from 'react';
import { Form, Input, Radio } from '@teamix/ui';

const FormItem = Form.Item;
const RadioGroup = Radio.Group;

const formItemLayout = {
  labelCol: {
    span: 6,
  },
  wrapperCol: {
    span: 14,
  },
};

const BasicDemo = () => {

  const userExists = async (rule, value) => {
    return new Promise((resolve, reject) => {
      if (!value) {
        resolve();
      } else {
        setTimeout(() => {
          if (value === 'frank') {
            reject([new Error('Sorry, this username is already exist.')]);
          } else {
            resolve();
          }
        }, 500);
      }
    });
  };

  return (
    <Form {...formItemLayout}>
      <FormItem label="Account:" hasFeedback validator={userExists} help="">
        <Input placeholder="Input frank" name="valUsername" />
        <Form.Error name="valUsername">
          {(errors, state) => {
            if (state === 'loading') {
              return 'loading...';
            } else {
              return errors;
            }
          }}
        </Form.Error>
      </FormItem>
      <FormItem label="Email:" hasFeedback required requiredTrigger="onBlur" format="email">
        <Input placeholder="Both trigget onBlur and onChange" name="valEmail" />
      </FormItem>

      <FormItem label="Password:" hasFeedback required requiredMessage="Please enter password">
        <Input type="password" name="valPasswd" />
      </FormItem>

      <FormItem label="Gender:" hasFeedback required requiredMessage="Please select your gender">
        <RadioGroup name="valSex">
          <Radio value="male">Male</Radio>
          <Radio value="female">Female</Radio>
        </RadioGroup>
      </FormItem>

      <FormItem label="Remarks:" required requiredMessage="Really do not intend to write anything?">
        <Input.TextArea
          maxLength={20}
          showLimitHint
          placeholder="Everything is ok!"
          name="valTextarea"
        />
      </FormItem>

      <FormItem wrapperCol={{ offset: 6 }}>
        <Form.Submit
          validate
          type="primary"
          onClick={(values) => console.log(values)}
          style={{ marginRight: 10 }}
        >
          Submit
        </Form.Submit>
        <Form.Reset>Reset</Form.Reset>
      </FormItem>
    </Form>
  );
};

export default () => <BasicDemo />;

```
## label 作为校验提示
```tsx
/**
 * title: label作为校验提示
 * description: 使用 label 作为校验提示
 */
import React from 'react';
import { Form, Input, Radio } from '@teamix/ui';

const FormItem = Form.Item;
const RadioGroup = Radio.Group;

const formItemLayout = {
  labelCol: {
    span: 6,
  },
  wrapperCol: {
    span: 14,
  },
};

const Demo = () => {

  return (
    <Form
      {...formItemLayout}
      useLabelForErrorMessage
      colon
    >
      <FormItem label="Account" required>
        <Input placeholder="Input frank" name="valUsername" />
      </FormItem>
      <FormItem
        label="Email"
        required
        requiredTrigger="onBlur"
        format="email"
      >
        <Input
          placeholder="Both trigget onBlur and onChange"
          name="valEmail"
        />
      </FormItem>

      <FormItem
        label="Password"
        hasFeedback
        required
        requiredMessage="Please enter password"
      >
        <Input htmlType="password" name="valPasswd" />
      </FormItem>

      <FormItem
        label="Gender"
        hasFeedback
        required
        requiredMessage="Please select your gender"
      >
        <RadioGroup name="valSex">
          <Radio value="male">Male</Radio>
          <Radio value="female">Female</Radio>
        </RadioGroup>
      </FormItem>

      <FormItem
        label="Remarks"
        required
        requiredMessage="Really do not intend to write anything?"
      >
        <Input.TextArea
          maxLength={20}
          showLimitHint
          placeholder="Everything is ok!"
          name="valTextarea"
        />
      </FormItem>

      <FormItem wrapperCol={{ offset: 6 }}>
        <Form.Submit
          validate
          type="primary"
          onClick={(v, e) => console.log(v, e)}
          style={{ marginRight: 10 }}
        >
          Submit
        </Form.Submit>
        <Form.Reset>Reset</Form.Reset>
      </FormItem>
    </Form>
  );
};

export default Demo;
```
## 复杂功能(Field)
```tsx
/**
 * title: 复杂功能(Field)
 * description: 配合 `Field` 可以实现较复杂功能
 */

import React from 'react';

import { Form, Input, Radio, Field, Button } from '@teamix/ui';

const FormItem = Form.Item;
const RadioGroup = Radio.Group;

const formItemLayout = {
  labelCol: {
    span: 6,
  },
  wrapperCol: {
    span: 14,
  },
};

const BasicDemo = () => {
  const field = Field.useField();

  const userExists = (rule, value) => {
    return new Promise((resolve, reject) => {
      if (!value) {
        resolve();
      } else {
        setTimeout(() => {
          if (value === 'frank') {
            reject([new Error('Sorry, this username is already occupied.')]);
          } else {
            resolve();
          }
        }, 500);
      }
    });
  }

  const checkPass = (rule, value, callback) => {
    const { validate } = field;
    if (value) {
      validate(['rePasswd']);
    }
    callback();
  }

  const checkPass2 = (rule, value, callback) => {
    const { getValue } = field;
    if (value && value !== getValue('passwd')) {
      return callback('Inconsistent password input twice!');
    } else {
      return callback();
    }
  }

  const validate = () => {
    field.validate(['sex']);
  };

    const { getState, getValue, getError } = field;

    return (
      <Form {...formItemLayout} field={field}>
        <FormItem
          label="Username:"
          hasFeedback
          required
          validator={userExists}
          help={
            getState('username') === 'loading'
              ? 'Checking ...'
              : getError('username')
          }
        >
          <Input placeholder="Input frank" name="username" />
          <p>Hello {getValue('username')}</p>
        </FormItem>

        <FormItem
          label="Password:"
          hasFeedback
          required
          requiredMessage="Please enter password"
          validator={checkPass}
        >
          <Input htmlType="password" name="passwd" />
        </FormItem>

        <FormItem
          label="Check your password:"
          hasFeedback
          required
          requiredMessage="Enter your password again"
          validator={checkPass2}
        >
          <Input
            htmlType="password"
            placeholder="Enter the same password twice"
            name="rePasswd"
          />
        </FormItem>

        <FormItem
          label="Gender:"
          hasFeedback
          required
          requiredMessage="Please select your gender"
        >
          <RadioGroup name="sex">
            <Radio value="male">Male</Radio>
            <Radio value="female">Female</Radio>
          </RadioGroup>
        </FormItem>

        <FormItem wrapperCol={{ offset: 6 }}>
          <Button onClick={validate}>Validate by Field</Button>
          <Form.Submit
            validate
            type="primary"
            onClick={(v, e) => console.log(v, e)}
            style={{ margin: '0 10px' }}
          >
            Submit
          </Form.Submit>
          <Form.Reset>Reset</Form.Reset>
        </FormItem>
      </Form>
    );
}

export default BasicDemo;
```
## 表单组合
```tsx
/**
 * title: 表单组合
 * description: 展示和表单相关的其他组件。
 */
import React from 'react';

import {
  Form,
  Input,
  Button,
  Checkbox,
  Radio,
  Select,
  Range,
  Balloon,
  DatePicker,
  TimePicker,
  NumberPicker,
  Field,
  Switch,
  Upload,
  Grid,
} from '@teamix/ui';

const FormItem = Form.Item;
const Option = Select.Option;
const RangePicker = DatePicker.RangePicker;
const { Row, Col } = Grid;

const formItemLayout = {
  labelCol: { span: 6 },
  wrapperCol: { span: 14 },
};

const Demo = () => {
  const field = Field.useField();

  const handleSubmit = (value) => {
    console.log('Form values：', value);
  };

  const init = field.init;

  return (
    <Form {...formItemLayout} field={field}>
      <FormItem label="I'm the title：">
        <p className="next-form-text-align">The quick brown fox jumps over the lazy dog.</p>
        <p>
          <a href="#">Link</a>
        </p>
      </FormItem>

      <FormItem label="Password:">
        <Balloon
          trigger={<Input htmlType="password" {...init('pass')} />}
          align="r"
          closable={false}
          triggerType="hover"
        >
          input password
        </Balloon>
      </FormItem>

      <FormItem label="NumberPicker:">
        <NumberPicker min={1} max={10} name="numberPicker" defaultValue={3} />
        <span>Something in here</span>
      </FormItem>

      <FormItem label="Switch:" required>
        <Switch name="switch" defaultChecked />
      </FormItem>

      <FormItem label="Range:" required>
        <Range defaultValue={30} scales={[0, 100]} marks={[0, 100]} name="range" />
      </FormItem>

      <FormItem label="Select:" required>
        <Select style={{ width: 200 }} name="select">
          <Option value="jack">jack</Option>
          <Option value="lucy">lucy</Option>
          <Option value="disabled" disabled>
            disabled
          </Option>
          <Option value="hugohua">hugohua</Option>
        </Select>
      </FormItem>

      <FormItem label="DatePicker:" labelCol={{ span: 6 }} required>
        <Row>
          <FormItem style={{ marginRight: 10, marginBottom: 0 }}>
            <DatePicker name="startDate" />
          </FormItem>
          <FormItem style={{ marginBottom: 0 }}>
            <DatePicker name="endDate" />
          </FormItem>
        </Row>
      </FormItem>

      <FormItem label="RangePicker:" labelCol={{ span: 6 }} required>
        <RangePicker name="rangeDate" />
      </FormItem>

      <FormItem label="TimePicker:" required>
        <TimePicker name="time" />
      </FormItem>

      <FormItem label="Checkbox:">
        <Checkbox.Group name="checkbox">
          <Checkbox value="a">option 1 </Checkbox>
          <Checkbox value="b">option 2 </Checkbox>
          <Checkbox disabled value="c">
            option 3（disabled）
          </Checkbox>
        </Checkbox.Group>
      </FormItem>

      <FormItem label="Radio:">
        <Radio.Group name="radio">
          <Radio value="apple">apple</Radio>
          <Radio value="banana">banana</Radio>
          <Radio disabled value="cherry">
            cherry（disabled）
          </Radio>
        </Radio.Group>
      </FormItem>

      <FormItem label="Logo：">
        <Upload action="/upload.do" listType="text" name="upload">
          <Button type="primary" style={{ margin: '0 0 10px' }}>
            Upload
          </Button>
        </Upload>
      </FormItem>
      <Row style={{ marginTop: 24 }}>
        <Col offset="6">
          <Form.Submit type="primary" onClick={handleSubmit}>
            Submit
          </Form.Submit>
        </Col>
      </Row>
    </Form>
  );
};

export default Demo;

```
## 移动端
```tsx
/**
 * title: 移动端
 * description: device=phone 下会强制设置 labelAlign=top
 */
import React, { useState } from 'react';
import { Form, Input, Checkbox, Switch, Radio } from '@teamix/ui';

const FormItem = Form.Item;

const formItemLayout = {
  labelCol: {
    fixedSpan: 10,
  },
  wrapperCol: {
    span: 14,
  },
};

const Demo = () => {
  const [device, setDevice] = useState('desktop');

  const handleDeviceChange = (device) => {
    setDevice(device);
  };

  return (
    <div>
      <Radio.Group
        shape="button"
        value={device}
        onChange={handleDeviceChange}
      >
        <Radio value="desktop">desktop</Radio>
        <Radio value="phone">phone</Radio>
      </Radio.Group>
      <hr />
      <Form
        style={{ width: '60%' }}
        {...formItemLayout}
        device={device}
      >
        <FormItem label="Username:">
          <p>Fixed Name</p>
        </FormItem>
        <FormItem label="password:">
          <Input
            htmlType="password"
            name="basePass"
            placeholder="Please Enter Password"
          />
        </FormItem>
        <FormItem label="Note:" help="something">
          <Input.TextArea placeholder="something" name="baseRemark" />
        </FormItem>
        <FormItem label="Agreement:">
          <Checkbox name="baseAgreement" defaultChecked>
            Agree
          </Checkbox>
        </FormItem>
        <FormItem label=" ">
          <Form.Submit>Confirm</Form.Submit>
        </FormItem>
      </Form>
    </div>
  );
};

export default Demo;
```
## 预览态
```tsx
/**
 * title: 预览态
 * description: 可以通过Form切换表单元素的预览态，切换前后布局结构相同
 */
import React, { useState } from 'react';
import {
  Form,
  Input,
  Switch,
  Rating,
  Icon,
  Radio,
  Range,
  Checkbox,
  NumberPicker,
  Select,
  Upload,
} from '@teamix/ui';

const FormItem = Form.Item;
const formItemLayout = {
  labelCol: {
    span: 7,
  },
  wrapperCol: {
    span: 16,
  },
};
const fileList = [
    {
      uid: '0',
      name: 'IMG.png',
      state: 'done',
      url: 'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
      downloadURL:
        'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
      imgURL:
        'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
    },
    {
      uid: '1',
      name: 'IMG.png',
      percent: 50,
      state: 'uploading',
      url: 'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
      downloadURL:
        'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
      imgURL:
        'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
    },
    {
      uid: '2',
      name: 'IMG.png',
      state: 'error',
      url: 'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
      downloadURL:
        'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
      imgURL:
        'https://img.alicdn.com/tps/TB19O79MVXXXXcZXVXXXXXXXXXX-1024-1024.jpg',
    },
  ];

const Demo = () => {
  const [preview, setPreview] = useState(false);

  const submitHandler = (e) => {
    console.log(e);
  };

  const onPreviewChange = (checked) => {
    setPreview(checked);
  };

  const ratingPreview = (value) => {
    return (
      <p>
        {value} {value > 2.5 ? <Icon type="smile" /> : <Icon type="cry" />}
      </p>
    );
  };

  return (
    <div>
      <Form
        {...formItemLayout}
        isPreview={preview}
        style={{ maxWidth: '800px' }}
      >
        <FormItem
          label="preview: "
          isPreview={false}
          size="small"
          style={{ marginBottom: 0 }}
        >
          <Switch size="large" onChange={onPreviewChange} />
        </FormItem>
        <div style={{ height: 1, width: '100%', margin: '20px 0' }} />
        <FormItem required label="Username:">
          <Input
            defaultValue="Fusion"
            placeholder="Please enter your username"
            id="username"
            name="username"
            aria-required="true"
          />
        </FormItem>
        <FormItem required label="Password:">
          <Input
            defaultValue="Fusion@2019"
            htmlType="password"
            placeholder="Please enter your password"
            id="password"
            name="password"
            aria-required="true"
          />
        </FormItem>

        <FormItem required label="Link:">
          <Input
            name="link"
            addonTextBefore="http://"
            addonTextAfter=".com"
            defaultValue="alibaba"
            aria-label="input with config of addonTextBefore and addonTextAfter"
          />
        </FormItem>

        <FormItem required label="Number:">
          <NumberPicker name="number" defaultValue={1} />
        </FormItem>

        <FormItem required label="autoComplete:">
          <Select.AutoComplete name="autoComplete" defaultValue="selected" />
        </FormItem>

        <FormItem required label="multiple Select:">
          <Select
            name="select"
            defaultValue={['apple', 'banana']}
            mode="multiple"
          >
            <Select.Option value="apple">Apple</Select.Option>
            <Select.Option value="banana">Banana</Select.Option>
          </Select>
        </FormItem>

        <FormItem required label="Rating:">
          <Rating
            defaultValue={4.5}
            name="rate"
            aria-label="what's the rate score"
          />
        </FormItem>

        <FormItem
          required
          label="Custom Render Rating:"
          renderPreview={ratingPreview}
        >
          <Rating
            defaultValue={4.5}
            name="rate2"
            aria-label="what's the rate2 score"
          />
        </FormItem>

        <FormItem required label="Checkbox:">
          <Checkbox.Group
            name="checkbox"
            defaultValue={['react', 'vue']}
          >
            <Checkbox value="react">React</Checkbox>
            <Checkbox value="vue">Vue</Checkbox>
            <Checkbox value="angular">Angular</Checkbox>
          </Checkbox.Group>
        </FormItem>

        <FormItem required label="Radio:">
          <Radio.Group name="radio" defaultValue="react">
            <Radio value="react">React</Radio>
            <Radio value="vue">Vue</Radio>
            <Radio value="angular">Angular</Radio>
          </Radio.Group>
        </FormItem>

        <FormItem required label="Range:">
          <Range
            name="range"
            slider="double"
            defaultValue={[10, 80]}
          />
        </FormItem>

        <FormItem label="Note:">
          <Input.TextArea
            placeholder="description"
            name="a11yRemark"
            defaultValue="Fusion 是一套企业级中后台UI的解决方案，致力于解决设计师与前端在产品体验一致性、工作协同、开发效率方面的问题。通过协助业务线构建设计系统，提供系统化工具协助设计师前端使用设计系统，下游提供一站式设计项目协作平台；打通互联网产品从设计到开发的工作流。"
          />
        </FormItem>

        <FormItem label="Upload:">
          <Upload name="upload" defaultValue={fileList} listType="text" />
        </FormItem>
        <FormItem label="Upload:">
          <Upload name="upload2" defaultValue={fileList} listType="image" />
        </FormItem>
        <FormItem wrapperCol={{ offset: 7 }}>
          <Form.Submit validate type="primary" onClick={submitHandler}>
            Submit
          </Form.Submit>
          <Form.Reset style={{ marginLeft: 10 }}>Reset</Form.Reset>
        </FormItem>
      </Form>
    </div>
  );
};

export default Demo;
```
## 无障碍支持
```tsx
/**
 * title: 无障碍支持
 * description: 对于必填项，置 `aria-required` 性，并通过视觉设计上的高亮提示用户。
 */
import React, { useState } from 'react';
import {
  Form,
  Input,
  Select,
  Radio,
  Checkbox,
  DatePicker,
  Switch,
  Upload,
  Grid,
  Field,
} from '@teamix/ui';

const RadioGroup = Radio.Group;
const { Row, Col } = Grid;
const FormItem = Form.Item;
const Option = Select.Option;
const formItemLayout = {
  labelCol: {
    span: 7,
  },
  wrapperCol: {
    span: 16,
  },
};

const Demo = () => {
  const [size, setSize] = useState('medium');

  const submitHandle = (e) => {
    console.log(e);
  };

  return (
    <div>
      <Form
        {...formItemLayout}
        size={size}
        style={{ maxWidth: '800px' }}
      >
        <FormItem required label="username:">
          <Input
            placeholder="Please enter your username"
            id="a11yUsername"
            name="a11yUsername"
            aria-required="true"
          />
        </FormItem>
        <FormItem required label="Password:">
          <Input
            htmlType="password"
            placeholder="Please enter your password"
            id="a11yPassword"
            name="a11yPassword"
            aria-required="true"
          />
        </FormItem>
        <FormItem
          id="myDateInput-1"
          required
          label="Accessible Date 1 (YYYY/MM/DD):"
          requiredMessage="Please select your date"
        >
          <DatePicker
            name="a11yDate"
            format="YYYY/MM/DD"
            inputProps={{ 'aria-required': 'true', id: 'myDateInput-1' }}
          />
        </FormItem>
        <FormItem
          required
          label="Accessible Date 2 (YYYY/MM/DD):"
          requiredMessage="Please select your date"
        >
          <DatePicker
            name="a11yOtherDate"
            format="YYYY/MM/DD"
            dateInputAriaLabel="Date input format YYYY/MM/DD"
            inputProps={{
              'aria-required': 'true',
              'aria-label': 'Accessible Date 2',
            }}
          />
        </FormItem>
        <FormItem label="Switch:">
          <Switch
            name="a11ySwitch"
            aria-label="Accessible Switch"
            defaultChecked
          />
        </FormItem>
        <FormItem
          required
          label="gender:"
          requiredMessage="Please select your gender"
        >
          <RadioGroup name="a11ySex">
            <Radio value="male" aria-required="true">
              Male
            </Radio>
            <Radio value="female" aria-required="true">
              Female
            </Radio>
          </RadioGroup>
        </FormItem>
        <FormItem label="Language:">
          <Checkbox.Group
            name="a11yLangs"
            aria-label="Please select a programming language"
          >
            <Checkbox value="python">python</Checkbox>
            <Checkbox value="java">java</Checkbox>
            <Checkbox value="angular">angular</Checkbox>
            <Checkbox value="c">c</Checkbox>
            <Checkbox value="other">other</Checkbox>
          </Checkbox.Group>
        </FormItem>
        <FormItem label="upload:">
          <Upload.Card
            listType="card"
            action="https://www.easy-mock.com/mock/5b713974309d0d7d107a74a3/alifd/upload"
            accept="image/png, image/jpg, image/jpeg, image/gif, image/bmp"
            defaultValue={[]}
            limit={2}
            name="a11yUpload"
          />
        </FormItem>
        <FormItem label="Note:">
          <Input.TextArea placeholder="description" name="a11yRemark" />
        </FormItem>
        <FormItem wrapperCol={{ offset: 7 }}>
          <Form.Submit
            validate
            type="primary"
            onClick={submitHandle}
            style={{ marginRight: 10 }}
          >
            Submit
          </Form.Submit>
          <Form.Reset>Reset</Form.Reset>
        </FormItem>
      </Form>
    </div>
  );
};

export default Demo;
```

## API

### Form

| 参数                    | 说明                                                                                                                                                                                                                                                                          | 类型            | 默认值                                             | 版本支持 |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | -------------------------------------------------- | -------- |
| inline                  | 内联表单                                                                                                                                                                                                                                                                      | Boolean         | -                                                  |          |
| size                    | 单个 Item 的 size 自定义，优先级高于 Form 的 size, 并且当组件与 Item 一起使用时，组件自身设置 size 属性无效。<br/><br/>**可选值**:<br/>'large'(大)<br/>'medium'(中)<br/>'small'(小)                                                                                           | Enum            | 'medium'                                           |          |
| fullWidth               | 单个 Item 中表单类组件宽度是否是 100%                                                                                                                                                                                                                                         | Boolean         | -                                                  |          |
| labelAlign              | 标签的位置, 如果不设置 labelCol 和 wrapperCol 那么默认是标签在上<br/><br/>**可选值**:<br/>'top'(上)<br/>'left'(左)<br/>'inset'(内)                                                                                                                                            | Enum            | 'left'                                             |          |
| labelTextAlign          | 标签的左右对齐方式<br/><br/>**可选值**:<br/>'left'(左)<br/>'right'(右)                                                                                                                                                                                                        | Enum            | -                                                  |          |
| field                   | field 实例, 传 false 会禁用 field                                                                                                                                                                                                                                             | any             | -                                                  |          |
| saveField               | 保存 Form 自动生成的 field 对象<br/><br/>**签名**:<br/>Function() => void                                                                                                                                                                                                     | Function        | func.noop                                          |          |
| labelCol                | 控制第一级 Item 的 labelCol                                                                                                                                                                                                                                                   | Object          | -                                                  |          |
| wrapperCol              | 控制第一级 Item 的 wrapperCol                                                                                                                                                                                                                                                 | Object          | -                                                  |          |
| onSubmit                | form 内有 `htmlType="submit"` 的元素的时候会触发<br/><br/>**签名**:<br/>Function() => void                                                                                                                                                                                    | Function        | function preventDefault(e) { e.preventDefault(); } |          |
| children                | 子元素                                                                                                                                                                                                                                                                        | any             | -                                                  |          |
| value                   | 表单数值                                                                                                                                                                                                                                                                      | Object          | -                                                  |          |
| onChange                | 表单变化回调<br/><br/>**签名**:<br/>Function(values: Object, item: Object) => void<br/>**参数**:<br/>_values_: {Object} 表单数据<br/>_item_: {Object} 详细<br/>_item.name_: {String} 变化的组件名<br/>_item.value_: {String} 变化的数据<br/>_item.field_: {Object} field 实例 | Function        | func.noop                                          |          |
| component               | 设置标签类型                                                                                                                                                                                                                                                                  | String/Function | 'form'                                             |          |
| device                  | 预设屏幕宽度<br/><br/>**可选值**:<br/>'phone', 'tablet', 'desktop'                                                                                                                                                                                                            | Enum            | 'desktop'                                          |          |
| responsive              | 是否开启内置的响应式布局 （使用 ResponsiveGrid）                                                                                                                                                                                                                              | Boolean         | -                                                  | 1.19     |
| isPreview               | 是否开启预览态                                                                                                                                                                                                                                                                | Boolean         | -                                                  | 1.19     |
| useLabelForErrorMessage | 是否使用 label 替换校验信息的 name 字段                                                                                                                                                                                                                                       | Boolean         | -                                                  | 1.20     |
| colon                   | 表示是否显示 label 后面的冒号                                                                                                                                                                                                                                                 | Boolean         | false                                              |          |

### Form.Item

> 手动传递了 wrapCol labelCol 会使用 Grid 辅助布局; labelAlign='top' 会强制禁用 Grid

| 参数                    | 说明                                                                                                                                                              | 类型               | 默认值 |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | ------ |
| label                   | label 标签的文本                                                                                                                                                  | ReactNode          | -      |
| size                    | 单个 Item 的 size 自定义，优先级高于 Form 的 size, 并且当组件与 Item 一起使用时，组件自身设置 size 属性无效。<br/><br/>**可选值**:<br/>'large', 'small', 'medium' | Enum               | -      |
| labelCol                | label 标签布局，通 `<Col>` 组件，设置 span offset 值，如 {span: 8, offset: 16}，该项仅在垂直表单有效                                                              | Object             | -      |
| wrapperCol              | 需要为输入控件设置布局样式时，使用该属性，用法同 labelCol                                                                                                         | Object             | -      |
| help                    | 自定义提示信息，如不设置，则会根据校验规则自动生成.                                                                                                               | ReactNode          | -      |
| extra                   | 额外的提示信息，和 help 类似，当需要错误信息和提示文案同时出现时，可以使用这个。 位于错误信息后面                                                                 | ReactNode          | -      |
| validateState           | 校验状态，如不设置，则会根据校验规则自动生成<br/><br/>**可选值**:<br/>'error'(失败)<br/>'success'(成功)<br/>'loading'(校验中)<br/>'warning'(警告)                 | Enum               | -      |
| hasFeedback             | 配合 validateState 属性使用，是否展示 success/loading 的校验状态图标, 目前只有 Input 支持                                                                         | Boolean            | false  |
| children                | node 或者 function(values)                                                                                                                                        | ReactNode/Function | -      |
| fullWidth               | 单个 Item 中表单类组件宽度是否是 100%                                                                                                                             | Boolean            | -      |
| labelAlign              | 标签的位置, 如果不设置 labelCol 和 wrapperCol 那么默认是标签在上<br/><br/>**可选值**:<br/>'top'(上)<br/>'left'(左)<br/>'inset'(内)                                | Enum               | -      |
| labelTextAlign          | 标签的左右对齐方式<br/><br/>**可选值**:<br/>'left'(左)<br/>'right'(右)                                                                                            | Enum               | -      |
| required                | [表单校验] 不能为空                                                                                                                                               | Boolean            | -      |
| asterisk                | required 的星号是否显示                                                                                                                                           | Boolean            | -      |
| requiredMessage         | required 自定义错误信息                                                                                                                                           | String             | -      |
| requiredTrigger         | required 自定义触发方式                                                                                                                                           | String/Array       | -      |
| min                     | [表单校验] 最小值                                                                                                                                                 | Number             | -      |
| max                     | [表单校验] 最大值                                                                                                                                                 | Number             | -      |
| minmaxMessage           | min/max 自定义错误信息                                                                                                                                            | String             | -      |
| minmaxTrigger           | min/max 自定义触发方式                                                                                                                                            | String/Array       | -      |
| minLength               | [表单校验] 字符串最小长度 / 数组最小个数                                                                                                                          | Number             | -      |
| maxLength               | [表单校验] 字符串最大长度 / 数组最大个数                                                                                                                          | Number             | -      |
| minmaxLengthMessage     | minLength/maxLength 自定义错误信息                                                                                                                                | String             | -      |
| minmaxLengthTrigger     | minLength/maxLength 自定义触发方式                                                                                                                                | String/Array       | -      |
| length                  | [表单校验] 字符串精确长度 / 数组精确个数                                                                                                                          | Number             | -      |
| lengthMessage           | length 自定义错误信息                                                                                                                                             | String             | -      |
| lengthTrigger           | length 自定义触发方式                                                                                                                                             | String/Array       | -      |
| pattern                 | 正则校验                                                                                                                                                          | any                | -      |
| patternMessage          | pattern 自定义错误信息                                                                                                                                            | String             | -      |
| patternTrigger          | pattern 自定义触发方式                                                                                                                                            | String/Array       | -      |
| format                  | [表单校验] 四种常用的 pattern<br/><br/>**可选值**:<br/>'number', 'email', 'url', 'tel'                                                                            | Enum               | -      |
| formatMessage           | format 自定义错误信息                                                                                                                                             | String             | -      |
| formatTrigger           | format 自定义触发方式                                                                                                                                             | String/Array       | -      |
| validator               | [表单校验] 自定义校验函数<br/><br/>**签名**:<br/>Function() => void                                                                                               | Function           | -      |
| validatorTrigger        | validator 自定义触发方式                                                                                                                                          | String/Array       | -      |
| autoValidate            | 是否修改数据时自动触发校验                                                                                                                                        | Boolean            | -      |
| device                  | 预设屏幕宽度<br/><br/>**可选值**:<br/>'phone', 'tablet', 'desktop'                                                                                                | Enum               | -      |
| colSpan                 | 在响应式布局模式下，表单项占多少列                                                                                                                                | Number             | -      |
| labelWidth              | 在响应式布局下，且 label 在左边时，label 的宽度是多少                                                                                                             | String/Number      | 100    |
| isPreview               | 是否开启预览态                                                                                                                                                    | Boolean            | -      |
| renderPreview           | 预览态模式下渲染的内容<br/><br/>**签名**:<br/>Function(value: any) => void<br/>**参数**:<br/>_value_: {any} 根据包裹的组件的 value 类型而决定                     | Function           | -      |
| useLabelForErrorMessage | 是否使用 label 替换校验信息的 name 字段                                                                                                                           | Boolean            | -      |
| colon                   | 表示是否显示 label 后面的冒号                                                                                                                                     | Boolean            | -      |
| valueName               | 子元素的 value 名称                                                                                                                                               | String             | -      |

### Form.Submit

> 继承 Button API

| 参数     | 说明                                                                                                                                                                                                   | 类型          | 默认值    |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | --------- |
| onClick  | 点击提交后触发<br/><br/>**签名**:<br/>Function(value: Object, errors: Object, field: class) => void<br/>**参数**:<br/>_value_: {Object} 数据<br/>_errors_: {Object} 错误数据<br/>_field_: {class} 实例 | Function      | func.noop |
| validate | 是否校验/需要校验的 name 数组                                                                                                                                                                          | Boolean/Array | -         |
| field    | 自定义 field (在 Form 内不需要设置)                                                                                                                                                                    | Object        | -         |

### Form.Reset

> 继承 Button API

| 参数      | 说明                                                     | 类型     | 默认值    |
| --------- | -------------------------------------------------------- | -------- | --------- |
| names     | 自定义重置的字段                                         | Array    | -         |
| onClick   | 点击提交后触发<br/><br/>**签名**:<br/>Function() => void | Function | func.noop |
| toDefault | 返回默认值                                               | Boolean  | -         |
| field     | 自定义 field (在 Form 内不需要设置)                      | Object   | -         |

### Form.Error

> 自定义错误展示

| 参数     | 说明                                                     | 类型               | 默认值 |
| -------- | -------------------------------------------------------- | ------------------ | ------ |
| name     | 表单名                                                   | String/Array       | -      |
| field    | 自定义 field (在 Form 内不需要设置)                      | Object             | -      |
| children | 自定义错误渲染, 可以是 node 或者 function(errors, state) | ReactNode/Function | -      |

## 说明
来自于https://gitee.com/tongyilingma/ui-components-wiki/blob/master/basic/form.md