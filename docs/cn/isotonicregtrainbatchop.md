## 功能介绍
保序回归在观念上是寻找一组非递减的片段连续线性函数（piecewise linear continuous functions），即保序函数，使其与样本尽可能的接近。
## 参数说明
<!-- This is the start of auto-generated parameter info -->
<!-- DO NOT EDIT THIS PART!!! -->
| 名称 | 中文名称 | 描述 | 类型 | 是否必须？ | 默认值 |
| --- | --- | --- | --- | --- | --- |
| featureCol | 特征列名 | 特征列的名称 | String |  | null |
| isotonic | 输出序列是否 | 输出序列是否递增 | Boolean |  | true |
| featureIndex | 训练特征所在维度 | 训练特征在输入向量的第featureIndex维 | Integer |  | 0 |
| labelCol | 标签列名 | 输入表中的标签列名 | String | ✓ |  |
| weightCol | 权重列名 | 权重列对应的列名 | String |  | null |
| vectorCol | 向量列名 | 向量列对应的列名，默认值是null | String |  | null |<!-- This is the end of auto-generated parameter info -->

## 脚本示例
#### 脚本代码
```python
data = np.array([[0.0, 0.3],
                [1.0, 0.27],
                [1.0, 0.55],
                [1.0, 0.5],
                [0.0, 0.2],
                [0.0, 0.18],
                [1.0, 0.1],
                [0.0, 0.45],
                [1.0, 0.9],
                [1.0, 0.8],
                [0.0, 0.7],
                [1.0, 0.6],
                [1.0, 0.35],
                [1.0, 0.4],
                [1.0, 0.02]])

df = pd.DataFrame({"feature" : data[:,0], "label" : data[:,1]})
data = dataframeToOperator(df, schemaStr="label double, feature double",op_type="batch")

trainOp = IsotonicRegTrainBatchOp()\
            .setFeatureCol("feature")\
			.setLabelCol("label")
model = trainOp.linkFrom(data)
predictOp = IsotonicRegPredictBatchOp().setPredictionCol("result")
predictOp.linkFrom(model, data).collectToDataframe()
```

#### 脚本运行结果
##### 模型结果
| model_id   | model_info |
| --- | --- |
| 0          | {"vectorCol":"\"col2\"","featureIndex":"0","featureCol":null} |
| 1048576    | [0.02,0.3,0.35,0.45,0.5,0.7] |
| 2097152    | [0.5,0.5,0.6666666865348816,0.6666666865348816,0.75,0.75] |
##### 预测结果
| col1       | col2       | col3       | pred       |
| --- | --- | --- | --- |
| 1.0        | 0.9        | 1.0        | 0.75       |
| 0.0        | 0.7        | 1.0        | 0.75       |
| 1.0        | 0.35       | 1.0        | 0.6666666865348816 |
| 1.0        | 0.02       | 1.0        | 0.5        |
| 1.0        | 0.27       | 1.0        | 0.5        |
| 1.0        | 0.5        | 1.0        | 0.75       |
| 0.0        | 0.18       | 1.0        | 0.5        |
| 0.0        | 0.45       | 1.0        | 0.6666666865348816 |
| 1.0        | 0.8        | 1.0        | 0.75       |
| 1.0        | 0.6        | 1.0        | 0.75       |
| 1.0        | 0.4        | 1.0        | 0.6666666865348816 |
| 0.0        | 0.3        | 1.0        | 0.5        |
| 1.0        | 0.55       | 1.0        | 0.75       |
| 0.0        | 0.2        | 1.0        | 0.5        |
| 1.0        | 0.1        | 1.0        | 0.5        |
