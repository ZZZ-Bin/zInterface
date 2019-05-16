# pagination

Vue 页码组件。

## props

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
​       <td>length</td>
​       <td>Number</td>
​       <td>-</td>
​       <td>true</td>
​     </tr>
​     <tr>
​       <td>last</td>
​       <td>Number</td>
​       <td>-</td>
​       <td>true</td>
​     </tr>
​     <tr>
​       <td>boundary</td>
​       <td>Boolean</td>
​				<td>false</td>
​				<td>false</td>
​			</tr>
​			<tr>
​				<td>theme</td>
​				<td>String</td>
​				<td>""</td>
​				<td>false</td>
​			</tr>
​		</tbody>
​	</table>

- length：显示的页码个数。
- last：页码总长度（尾页数）。
- boundary：是否显示 首/尾 页。
- theme：主题样式。

​ </br>

## theme
#### blunt:
![blunt](https://raw.githubusercontent.com/ZZZ-Bin/img-folder/master/zInterface/blunt.png?token=AICBY33MUDSTB7VHLGGTQMK43UVVW)

#### bubble-ching:
![bubble-ching](https://raw.githubusercontent.com/ZZZ-Bin/img-folder/master/zInterface/bubble-ching.png?token=AICBY37D7QDGCCJYGASSN7K43UVPC)

#### texture-dark
![texture-dark](https://raw.githubusercontent.com/ZZZ-Bin/img-folder/master/zInterface/texture-dark.png?token=AICBY35IWKAGP3OJ5OOTM7C43UVRU)

​ </br>
## page 监听

如需要，可在组件上监听 `current-page`:
```
      <Pagination @current-page='changePage' :setting='setting'></Pagination>

```
```
methods: {
  ...,
  changePage (page) {
    ...
  }
}
```
该参数为一个 `number` 值，表示组件当前的页码。

​</br>
