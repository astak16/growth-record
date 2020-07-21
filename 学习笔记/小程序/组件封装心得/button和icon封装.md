`lin-ui`组件学习心得：
- `l-button`组件，封装组件时，需要考虑多种情况
  - 原生组件提供的能力不能丢失
  - 内置样式可以当做属性传入`l-button-{{middle}}`，`middle`就是一个外部传入的属性。
  - 开发者可以自定义内容，且使用简单
    - 多个`slot`之间，可以用`if...else...`切换，这样能保证只有一个`slot`

    正常使用`slot`：
    ```ts
    <component-wrap>
      <view slot="a">uccs</view>
      <view slot="b">uccs</view>
    </component-wrap>

    // component-wrap  
    <view>
      <slot name="a" />
      <slot name="b" />
    </view>

    component({
      options: {
        multipleSlots: true   // 在组件定义时的选项中启用多slot支持
      }
    });
    ```

    进阶使用：
    ```ts
    // 使用方法 1
    <component-wrap>
      <view>uccs</view>
    </component-wrap>

    // 使用方法 2
    <component-wrap special="true">
      <view>uccs</view>
    </component-wrap>

    // compoment-wrap
    <view>
      <block wx:if="{{special}}">
        <view class="...">
          <slot />
        </view>
      </block>
      <block wx:else>
        <slot />
      </block>
    </view>

    Component({
      properties:{
       special: Boolean 
      }
    })
    ```
  - `l-icon`组件
    - 组件必传参数如果没有传递，需要抛出错误，可以在`attached`和`ready`中抛出错误
    ```ts
    Conpoment({
      name: String
      ready() {
        if(!this.data.name) return console.error("name 属性没有传递")
      }
    })
    
    // 或者
    Conpoment({
      name: String
      attached() {
        if(!this.data.name) return console.error("name 属性没有传递")
      }
    })
    ```
