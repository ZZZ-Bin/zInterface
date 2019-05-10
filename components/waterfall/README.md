# waterfall ![瘦小的圆角矩形](https://img.shields.io/badge/Vue-v2.6.0+-brightgreen.svg)

Vue 瀑布流组件。组件接收两 props: [urls](#user-content---urls包含图片-url-的数组)、[setting](#user-content---setting-组件相关设置)，并预留了一些 [slots](#slots) 以便添加一些[自定义功能](#user-content---示例)或对组件进行扩展。

## props

#### \- urls：*包含图片 `url` 的数组。*
  <table>
​   <tbody align="center">
​     <tr>
​       <th>type</th>
​       <th>default</th>
​       <th>required</th>
​     </tr>
​     <tr>
​       <td>Array</td>
​       <td>[]</td>
​       <td>false</td>
​     </tr>
​   </tbody>
​ </table>
​ </br>

#### \- setting: *组件相关设置。*
  <table>
​   <tbody align="center" size="12px">
​     <tr>
​       <th>key</th>
​       <th>type</th>
​       <th>default</th>
​       <th>required</th>
​     </tr>
​     <tr>
​       <td>colNum</td>
​       <td>Number</td>
​       <td>-</td>
​       <td>true</td>
​     </tr>
​     <tr>
​       <td>colWidth</td>
​       <td>String</td>
​       <td>-</td>
​       <td>true</td>
​     </tr>
​     <tr>
​       <td>margin</td>
​       <td>String</td>
​				<td>""</td>
​				<td>false</td>
​			</tr>
​			<tr>
​				<td>fill</td>
​				<td>String</td>
​				<td>""</td>
​				<td>false</td>
​			</tr>
​			<tr>
​				<td>failImg</td>
​				<td>String</td>
​				<td>""</td>
​				<td>false</td>
​			</tr>
​			<tr>
​				<td>loadingImg</td>
​				<td>String</td>
​				<td>""</td>
​				<td>false</td>
​			</tr>
​		</tbody>
<<<<<<< HEAD


​	</table>



=======
​	</table>
<<<<<<< HEAD
>>>>>>> c6863e44fe12aebb44c9a26586df0c8e8038df69
=======

- colNum：图片列数。
- colWidth：列宽度，接收带单位尺寸字符串，(`“10px”`、`"20%"`)。
- margin：图片间距，只接收像素尺寸，并自动转化为百分比尺寸。
- fill：自动补全，接收 `rgb 颜色值`和`十六进制颜色码`，默认值 `""` 不开启补全。
- failImg：加载失败替换图片，默认值 `""` 加载失败的图片不会被添加到列中。
- loadingImg：加载中图片。

​ </br>
## slots

组件在不同位置预留了三个插槽，以便在使用时根据需求对组件进行扩展。

**\- cell**
图片单元插槽，图片路径被保存在 `"url"` 中。

**\- group**
列插槽

**\- whole**
整体插槽

​</br>
## 示例

通过组件预留的插槽，在使用过程中可对组件进行内容和功能上的扩展，以下展示部分功能的添加，你可以根据需要自行扩展组件。

#### 添加蒙版、点击放大图片
##### template：
```
  ...
  <Waterfall :urls='datas' :setting='setting'>
    <template #cell='data'>
        <div class='mask'
             @click='enlarge(data.url)'>
          <p>点击放大图片</p>
        </div>
      </template>
  <Waterfall>
  <div class='magnifier'
       v-if='magnifierImage'
       @click='enlarge'>
    <img :src='magnifierImage'
         alt="">
  </div>
  ...
```

##### script:
```
  ...
  data: function () {
    return {
      ...,
      magnifierImage: ''
    }
  },
  methods: {
    ...,
    enlarge (url) {
      if (this.magnifierImage === '') this.magnifierImage = url
      else this.magnifierImage = ''
    }
  }
```

##### style：
```
  .mask {
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    opacity: 0;
    transition: all 0.25s;
  }
  .mask:hover {
    opacity: 1;
  }
```
>>>>>>> c6f5fe7e50492c99b2f7470b24164c473da96621
