# destiny-2-bot-added-deepseek
import requests
import json

messages = [{"role": "system",
"content":
'''
你是一个专业的命运：群星游戏专家，精通游戏的所有方面。请用中文回答，回答要：
        1. 准确、详细但简洁
        2. 包含实用的攻略和技巧
        3. 如果是武器，要说明获取方式和推荐perk
        4. 如果是副本，要说明机制和打法
        5. 不查询游戏玩家个人信息内容
        6. 信息具有时效性,当前赛季为宿命边缘
'''}
]

url = "https://api.deepseek.com/chat/completions"
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'Authorization': 'Bearer sk-7396d4f233e54902966769c5b4ebacaf'
}

def content():
  response = requests.request("POST", url, headers=headers, data=data)
  response = json.loads(response.text)#将response.text（为json）内容解码为python类型
  message = response['choices']#choices是列表
  message = message[0]#转化为字典
  message = message['message']#提取其中message内容，,也是系统回复内容,字典类型,内容{'role': 'assistant', 'content':'','reasoning_content':}
  content = message['content']#系统回复内容
  reasoning_content = message['reasoning_content']#系统思考内容
  del message['reasoning_content']
  messages.append(message)#将系统回复内容添加到messages末尾，进行对论对对话准备
  # print('响应内容（py）', response)
  # print('message内容', message)
  # print('传入messages序列：', messages)
  print('思考内容：', reasoning_content)  # 思考内容
  print('对话内容：', content)  # 对话内容
#注：多轮对话不能把reasoning_content传入messages 序列

while True:

  user_input = input('用户:')
  messages.append({"content": user_input, "role": "user"})#提问内容

  data = json.dumps({
    "messages": messages,
    "model": "deepseek-reasoner",
    "frequency_penalty": 1,
    "max_tokens": 32768,
    "presence_penalty": 0,
    "response_format": {
      "type": "text"
    },
    "stop": None,
    "stream": False,
    "stream_options": None,
    "temperature": 1,
    "top_p": 1,
    "tools": None,
    "tool_choice": "none",
    "logprobs": False,
    "top_logprobs": None
  })

  content()

  if user_input == '退出':
    break




'''
response.text内容
{'id': 'ec9c2191-b412-4405-a2d3-184a16699a3f',
 'object': 'chat.completion', 创建聊天完成时的 Unix 时间戳（以秒为单位）。
 'created': 1760272141,
  'model': 'deepseek-chat', 
  
'choices': [{'index': 0, 
'message': {'role': 'assistant', 'content': "Hello! 👋 How can I help you today? Feel free to ask me anything—I'm here to assist! 😊"}, 
'logprobs': None, 'finish_reason': 'stop'}], 

'usage': {'prompt_tokens': 10, 用户 prompt 所包含的 token 数。该值等于 prompt_cache_hit_tokens + prompt_cache_miss_tokens
'completion_tokens': 26, 模型 completion 产生的 token 数。
'total_tokens': 36, 该请求中，所有 token 的数量（prompt + completion）。
'prompt_tokens_details': {'cached_tokens': 0}, 
'prompt_cache_hit_tokens': 0,'prompt_cache_miss_tokens': 10},用户 prompt 中，命中上下文缓存的 token 数。
'system_fingerprint': 'fp_ffc7281d48_prod0820_fp8_kvcache'}该指纹代表模型运行时采用的后端配置。
prompt_cache_miss_tokens：用户 prompt 中，未命中上下文缓存的 token 数。

completion tokens 的详细信息：
reasoning_tokens：
推理模型所产生的思维链 token 数量
'''

# print(respondse)#respondse.text(返回响应的内容，unicode 类型数据),解码为py类型
