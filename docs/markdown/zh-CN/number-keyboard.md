## NumberKeyboard 数字键盘

### 使用指南
``` javascript
import { NumberKeyboard } from 'vant';

Vue.use(NumberKeyboard);
```

### 代码演示

#### 默认样式

```html
<van-button @touchstart.native.stop="showKeyboard = true">
  弹出默认键盘
</van-button>

<van-number-keyboard
  extraKey="."
  :show="showKeyboard"
  closeButtonText="完成"
  @blur="showKeyboard = false"
  @input="onInput"
  @delete="onDelete"
/>
```

```javascript
export default {
  data() {
    return {
      showKeyboard: true
    }
  },

  methods: {
    onInput(value) {
      Toast(value);
    },
    onDelete() {
      Toast('delete');
    }
  }
}
```

#### 自定义样式

```html
<van-number-keyboard
  theme="custom"
  extraKey="."
  :show="showKeyboard"
  closeButtonText="完成"
  @blur="showKeyboard = false"
  @input="onInput"
  @delete="onDelete"
/>
```

### API

| 参数 | 说明 | 类型 | 默认值 | 可选值 |
|-----------|-----------|-----------|-------------|-------------|
| show | 是否显示键盘 | `Boolean` | - | - |
| theme | 键盘样式风格 | `String` | `Default` | `Custom` |
| title | 键盘标题 | `String` | - | - |
| zIndex | 键盘 z-index | `Number` | `100` | - |
| extraKey | 左下角按键内容 | `String` | `''` | - |
| closeButtonText | 关闭按钮文字，空则不展示 | `String` | `-` | - |
| transition | 是否开启过场动画 | `Boolean` | `true` | - |
| showDeleteKey | 是否展示删除按钮 | `Boolean` | `true` | - |
| hideOnClickOutside | 点击外部时是否收起键盘 | `Boolean` | `true` | - |

### Event

| 事件名 | 说明 | 参数 |
|-----------|-----------|-----------|
| input | 点击按键时触发 | key: 按键内容 |
| delete | 点击删除键时触发 | - |
| blur | 点击非键盘区域时触发 | - |
| show | 键盘完全弹出时触发 | - |
| hide | 键盘完全收起时触发 | - |
