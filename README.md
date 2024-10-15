> https://cloud.baidu.com/doc/WENXINWORKSHOP/s/clntwmv7t#%E8%AF%B7%E6%B1%82%E8%AF%B4%E6%98%8E
## URL

请求地址： `https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop/chat/completions_pro`

请求方式： POST

## Body参数

| 名称                 | 类型          | 必填 | 描述                                                         |
| -------------------- | ------------- | ---- | ------------------------------------------------------------ |
| messages             | List(message) | 是   | 聊天上下文信息。说明： （1）messages成员不能为空，1个成员表示单轮对话，多个成员表示多轮对话，例如： · 1个成员示例，`"messages": [ {"role": "user","content": "你好"}]` · 3个成员示例，`"messages": [ {"role": "user","content": "你好"},{"role":"assistant","content":"需要什么帮助"},{"role":"user","content":"自我介绍下"}]` （2）最后一个message为当前请求的信息，前面的message为历史对话信息 （3）成员数目必须为奇数，成员中message的role值说明如下：奇数位message的role值必须为user，偶数位message的role值为assistant。例如： 示例中message中的role值分别为user、assistant、user、assistant、user；奇数位（红框）message中的role值为user，即第1、3、5个message中的role值为user；偶数位（蓝框）值为assistant，即第2、4个message中的role值为assistant ![image.png](https://bce.bdstatic.com/doc/ai-cloud-share/WENXINWORKSHOP/image_e3d47d5.png) （4）message中的content总长度和system字段总内容不能超过20000个字符，且不能超过5120 tokens |
| temperature          | float         | 否   | 说明： （1）较高的数值会使输出更加随机，而较低的数值会使其更加集中和确定 （2）默认0.8，范围 (0, 1.0]，不能为0 |
| top_p                | float         | 否   | 说明： （1）影响输出文本的多样性，取值越大，生成文本的多样性越强 （2）默认0.8，取值范围 [0, 1.0] |
| penalty_score        | float         | 否   | 通过对已生成的token增加惩罚，减少重复生成的现象。说明： （1）值越大表示惩罚越大 （2）默认1.0，取值范围：[1.0, 2.0] |
| stream               | bool          | 否   | 是否以流式接口的形式返回数据，默认false                      |
| enable_system_memory | bool          | 否   | 是否开启系统记忆，说明： （1）false：未开启，默认false （2）true：表示开启，开启后，system_memory_id字段必填 |
| system_memory_id     | string        | 否   | 系统记忆ID，说明： （1）用于读取对应ID下的系统记忆，读取到的记忆文本内容会拼接message参与请求推理 （2）通过调用[创建系统记忆](https://cloud.baidu.com/doc/WENXINWORKSHOP/s/Mlwg321zw)接口，返回的result字段获取 |
| system               | string        | 否   | 模型人设，主要用于人设设定，例如，你是xxx公司制作的AI助手，说明： （1）长度限制，message中的content总长度和system字段总内容不能超过20000个字符，且不能超过5120 tokens |
| stop                 | List(string)  | 否   | 生成停止标识，当模型生成结果以stop中某个元素结尾时，停止文本生成。说明： （1）每个元素长度不超过20字符 （2）最多4个元素 |
| disable_search       | bool          | 否   | 是否强制关闭实时搜索功能，可选值： · true：关闭 · false：表示不关闭，默认false |
| enable_citation      | bool          | 否   | 是否开启上角标返回，说明： （1）开启后，有概率触发搜索溯源信息search_info，search_info内容见响应参数介绍 （2）默认false，不开启 （3）注意：本字段不控制是否关闭实时搜索功能，该功能请参见disable_search字段。如果设置了disable_search=true，那么本字段无效 |
| enable_trace         | bool          | 否   | 是否返回搜索溯源信息，说明： （1）如果开启，在触发了搜索增强的场景下，会返回搜索溯源信息search_info，search_info内容见响应参数介绍 （2）默认false，表示不开启 （3）注意：本字段不控制是否关闭实时搜索功能，该功能请参见disable_search字段。如果设置了disable_search=true，那么本字段无效 |
| max_output_tokens    | int           | 否   | 指定模型最大输出token数，说明： （1）如果设置此参数，范围[2, 2048] （2）如果不设置此参数，最大输出token数为1024 |
| response_format      | string        | 否   | 指定响应内容的格式，说明： （1）可选值： · json_object：以json格式返回，可能出现不满足效果情况 · text：以文本格式返回 （2）如果不填写参数response_format值，默认为text |
| user_ip              | string        | 否   | 用户IP，可用于推测用户位置等                                 |
| user_id              | string        | 否   | 表示最终用户的唯一标识符                                     |



**message说明**

| 名称    | 类型   | 必填 | 描述                                                  |
| ------- | ------ | ---- | ----------------------------------------------------- |
| role    | string | 是   | 当前支持以下： user: 表示用户 assistant: 表示对话助手 |
| content | string | 是   | 对话内容                                              |
| name    | string | 否   | message作者                                           |



## 响应说明

- **响应头Header参数**

| 名称                           | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| X-Ratelimit-Limit-Requests     | 一分钟内允许的最大请求次数                                   |
| X-Ratelimit-Limit-Tokens       | 一分钟内允许的最大tokens消耗，包含输入tokens和输出tokens     |
| X-Ratelimit-Remaining-Requests | 达到RPM速率限制前，剩余可发送的请求数配额，如果配额用完，将会在0-60s后刷新 |
| X-Ratelimit-Remaining-Tokens   | 达到TPM速率限制前，剩余可消耗的tokens数配额，如果配额用完，将会在0-60s后刷新 |

- **响应体参数**

| 名称               | 类型        | 描述                                                         |
| ------------------ | ----------- | ------------------------------------------------------------ |
| id                 | string      | 本轮对话的id                                                 |
| object             | string      | 回包类型 chat.completion：多轮对话返回                       |
| created            | int         | 时间戳                                                       |
| sentence_id        | int         | 表示当前子句的序号。只有在流式接口模式下会返回该字段         |
| is_end             | bool        | 表示当前子句是否是最后一句。只有在流式接口模式下会返回该字段 |
| is_truncated       | bool        | 当前生成的结果是否被截断                                     |
| finish_reason      | string      | 输出内容标识，说明： · normal：输出内容完全由大模型生成，未触发截断、替换 · stop：输出结果命中入参stop中指定的字段后被截断 · length：达到了最大的token数，根据EB返回结果is_truncated来截断 · content_filter：输出内容被截断、兜底、替换为**等 |
| search_info        | search_info | 搜索数据，当请求参数enable_citation或enable_trace为true，并且触发搜索时，会返回该字段 |
| result             | string      | 对话返回结果                                                 |
| need_clear_history | bool        | 表示用户输入是否存在安全风险，是否关闭当前会话，清理历史会话信息 true：是，表示用户输入存在安全风险，建议关闭当前会话，清理历史会话信息 false：否，表示用户输入无安全风险 |
| flag               | int         | 说明：返回flag表示触发安全                                   |
| ban_round          | int         | 当need_clear_history为true时，此字段会告知第几轮对话有敏感信息，如果是当前问题，ban_round=-1 |
| usage              | usage       | token统计信息                                                |

**search_info说明**

| 名称           | 类型                | 描述         |
| -------------- | ------------------- | ------------ |
| search_results | List(search_result) | 搜索结果列表 |

**search_result说明**

| 名称  | 类型   | 描述         |
| ----- | ------ | ------------ |
| index | int    | 序号         |
| url   | string | 搜索结果URL  |
| title | string | 搜索结果标题 |

**usage说明**

| 名称                  | 类型   | 描述                                                         |
| --------------------- | ------ | ------------------------------------------------------------ |
| prompt_tokens         | int    | 问题tokens数，说明： （1）如果未触发独立搜索，指用户输入原始问题tokens数 （2）如果触发独立搜索，问题tokens数包括用户输入问题token数和触发搜索相关的tokens数 |
| completion_tokens     | int    | 回答tokens数                                                 |
| total_tokens          | int    | tokens总数                                                   |
| prompt_tokens_details | Object | 问题tokens数详情                                             |
| search_count          | int    | 触发独立搜索计费次数                                         |

**prompt_tokens_details说明**

| 名称          | 类型 | 描述                 |
| ------------- | ---- | -------------------- |
| search_tokens | int  | 搜索独立计费tokens数 |
