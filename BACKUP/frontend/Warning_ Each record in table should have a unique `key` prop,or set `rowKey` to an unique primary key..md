# Warning_ Each record in table should have a unique `key` prop,or set `rowKey` to an unique primary key.

# Warning_ Each record in table should have a unique `key` prop,or set `rowKey` to an unique primary key.

antD表格警告：warning.js?2149:7 Warning: [antdv: Each record in table should have a unique

问题：antD控制台警告（Warning: [antdv: Each record in table should have a unique `key` prop,or set `rowKey` to an unique primary key.] ）

回答：table里加rowKey={(record, index) => index}

```javascript
<Table
   dataSource={data || []}
   size="small"
   tableLayout="fixed"
   rowKey={(record, index) => index}//解决antd表格警告
>
```
