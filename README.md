<div align="center">
<h1 align="center">Search RAG</h1>
<h2 align="center">search_with_lepton [ https://github.com/leptonai/search_with_lepton ] 的自部署版</h2>
Build your own conversational search engine using less than 500 lines of code.
<br/>
<img width="70%" src="https://github.com/leptonai/search_with_lepton/assets/1506722/845d7057-02cd-404e-bbc7-60f4bae89680">
</div>

## 启动后端服务（运行命令）
docker run \
--shm-size=64g \
--name my_xinference -d -p 9997:9997 -e XINFERENCE_MODEL_SRC=modelscope \
-v /home/jqsoft/.xinference:/root/.xinference \
-v /home/jqsoft/.cache/huggingface:/root/.cache/huggingface \
-v /home/jqsoft/.cache/modelscope:/root/.cache/modelscope \
--gpus all xprobe/xinference:revised xinference-local -H 0.0.0.0 --log-level debug

CUDA_VISIBLE_DEVICES=0,1,2,3 xinference launch --model-name qwen1.5-chat --size-in-billions 72 --n_gpu 4 --model-format gptq --quantization Int4  --gpu_memory_utilization 0.85 --tensor_parallel_size 4 --log-level debug

## 运行my_search_rag.py

## 下面是源repository[ https://github.com/leptonai/search_with_lepton ]  的readme内容
## Features
- Built-in support for LLM
- Built-in support for search engine
- Customizable pretty UI interface
- Shareable, cached search results

## Setup Search Engine API
There are two default supported search engines: Bing and Google.
 
### Bing Search
To use the Bing Web Search API, please visit [this link](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api) to obtain your Bing subscription key.

### Google Search
You have three options for Google Search: you can use the [SearchApi Google Search API](https://www.searchapi.io/) from SearchApi, [Serper Google Search API](https://www.serper.dev) from Serper, or opt for the [Programmable Search Engine](https://developers.google.com/custom-search) provided by Google.

## Setup LLM and KV

> [!NOTE]
> We recommend using the built-in llm and kv functions with Lepton. 
> Running the following commands to set up them automatically.

```shell
pip install -U leptonai && lep login
```


## Build

1. Set Bing subscription key
```shell
export BING_SEARCH_V7_SUBSCRIPTION_KEY=YOUR_BING_SUBSCRIPTION_KEY
```
2. Build web
```shell
cd web && npm install && npm run build
```
3. Run server
```shell
BACKEND=BING python search_with_lepton.py
```

For Google Search using SearchApi:
```shell
export SEARCHAPI_API_KEY=YOUR_SEARCHAPI_API_KEY
BACKEND=SEARCHAPI python search_with_lepton.py
```

For Google Search using Serper:
```shell
export SERPER_SEARCH_API_KEY=YOUR_SERPER_API_KEY
BACKEND=SERPER python search_with_lepton.py
```

For Google Search using Programmable Search Engine:
```shell
export GOOGLE_SEARCH_API_KEY=YOUR_GOOGLE_SEARCH_API_KEY
export GOOGLE_SEARCH_CX=YOUR_GOOGLE_SEARCH_ENGINE_ID
BACKEND=GOOGLE python search_with_lepton.py
```



## Deploy

You can deploy this to Lepton AI with one click:

[![Deploy with Lepton AI](https://github.com/leptonai/search_with_lepton/assets/1506722/bbd40afa-69ee-4acb-8974-d060880a183a)](https://dashboard.lepton.ai/workspace-redirect/explore/detail/search-by-lepton)

You can also deploy your own version via

```shell
lep photon run -n search-with-lepton-modified -m search_with_lepton.py --env BACKEND=BING --env BING_SEARCH_V7_SUBSCRIPTION_KEY=YOUR_BING_SUBSCRIPTION_KEY
```

Learn more about `lep photon` [here](https://www.lepton.ai/docs).
