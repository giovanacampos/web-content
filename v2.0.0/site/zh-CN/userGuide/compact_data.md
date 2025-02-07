---
id: compact_data.md
related_key: compact data
summary: Learn how to compact data in Milvus.
---

# Compact Data

<div class="alert note">
<h3>Milvus Docs 需要你的帮助</h3>
本文档暂时没有中文版本，欢迎你成为社区贡献者，协助中文技术文档的翻译。<br>
你可以通过页面右边的 <b>编辑</b> 按钮直接贡献你的翻译。更多详情，参考 <a href="https://github.com/milvus-io/milvus-docs/blob/v2.0.0/CONTRIBUTING.md">贡献指南</a>。如需帮助，你可以 <a href="https://github.com/milvus-io/milvus-docs/issues/new/choose">提交 GitHub Issue</a>。
</div>


This topic describes how to compact data in Milvus.

Milvus supports automatic data compaction by default. You can [configure](configure-docker.md) your Milvus to enable or disable [compaction](configure_datacoord.md#dataCoordenableCompaction) and [automatic compaction](configure_datacoord.md#dataCoordcompactionenableAutoCompaction).

If automatic compaction is disabled, you can still compact data manually.

<div class="alert note">
To ensure accuracy of searches with Time Travel, Milvus retains the data operation log within the span specified in <a href="configure_common.md#common.retentionDuration"><code>common.retentionDuration</code></a>. Therefore, data operated within this period will not be compacted. 
</div>

## Compact data manually

Compaction requests are processed asynchronously because they are usually time-consuming. 

<div class="multipleCode">
  <a href="?python">Python </a>
  <a href="?java">Java</a>
  <a href="?go">GO</a>
  <a href="?javascript">Node.js</a>
  <a href="?shell">CLI</a>
</div>


```python
from pymilvus import Collection
collection = Collection("book")      # Get an existing collection.
collection.compact()
```

```javascript
const res = await milvusClient.collectionManager.compact({
  collection_name: "book",
});
const compactionID = res.compactionID;
```

```go
// This function is under active development on the GO client.
```

```java
R<ManualCompactionResponse> response = milvusClient.manualCompaction(
    ManualCompactionParam.newBuilder()
        .withCollectionName("book")
        .build());
long compactionID = response.getData().getCompactionID();
```

```shell
compact -c book
```

<table class="language-javascript">
	<thead>
	<tr>
		<th>Parameter</th>
		<th>Description</th>
	</tr>
	</thead>
	<tbody>
	<tr>
		<td><code>collection_name</code></td>
		<td>Name of the collection to compact data.</td>
	</tr>
	</tbody>
</table>

<table class="language-java">
	<thead>
        <tr>
            <th>Parameter</th>
            <th>Description</th>
        </tr>
	</thead>
	<tbody>
        <tr>
            <td><code>CollectionName</code></td>
            <td>Name of the collection to compact data.</td>
        </tr>
    </tbody>
</table>

<table class="language-shell">
    <thead>
        <tr>
            <th>Option</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-c</td>
            <td>Name of the collection to compact data.</td>
        </tr>
    </tbody>
</table>

## Check compaction status

You can check the compaction status with the compaction ID returned when the manual compaction is triggered.

<div class="multipleCode">
  <a href="?python">Python </a>
  <a href="?java">Java</a>
  <a href="?go">GO</a>
  <a href="?javascript">Node.js</a>
  <a href="?shell">CLI</a>
</div>


```python
collection.get_compaction_state()
```

```javascript
const state = await milvusClient.collectionManager.getCompactionState({
    compactionID
});
```

```go
// This function is under active development on the GO client.
```

```java
milvusClient.getCompactionState(GetCompactionStateParam.newBuilder()
        .withCompactionID(compactionID)
        .build());
```

```shell
show compaction_state -c book
```

## What's next

- Learn more basic operations of Milvus:
  - [Insert data into Milvus](insert_data.md)
  - [Create a partition](create_partition.md)
  - [Build an index for vectors](build_index.md)
  - [Conduct a vector search](search.md)
  - [Conduct a hybrid search](hybridsearch.md)
- Explore API references for Milvus SDKs:
  - [PyMilvus API reference](/api-reference/pymilvus/v2.0.1/tutorial.html)
  - [Node.js API reference](/api-reference/node/v2.0.1/tutorial.html)

