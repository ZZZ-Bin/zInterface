# waterfall

Vue 瀑布流组件。组件接收两 `props`: [urls](#user-content---urls包含图片-url-的数组)、[setting](#user-content---setting-组件相关设置)，并预留了一些 [slots](#slots) 以便添加一些自定义功能或对组件进行扩展。

## props

#### \- urls：*包含图片 url 的数组。*
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
​	</table>

- colNum：图片列数。
- colWidth：列宽度，接收带单位尺寸字符串，如：`“10px”`、`"20%"`。
- margin：图片下间距，只接收像素尺寸，并自动转化为百分比尺寸。
- fill：自动补全，接收 `rgb 颜色值`和`十六进制颜色码`，默认值 `""` 不开启补全。
- failImg：加载失败替换图片，默认值 `""` 加载失败的图片不会被添加到列中。
- loadingImg：加载中图片。

​ </br>
## slots

组件在不同位置预留了三个插槽，以便在使用时根据需求对组件进行扩展。

**\- cell**

**\- group**

**\- whole**

