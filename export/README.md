# llm-export
English
llm-export is a tool for exporting LLM models, capable of converting LLM models to ONNX and MNN formats.
* 🚀 Optimized original code to support dynamic shapes
* 🚀 Optimized original code to reduce constant parts
* 🚀 Used OnnxSlim to optimize ONNX models, improving performance by about 5%; by @inisis
* 🚀 Supports exporting LoRA weights to ONNX and MNN
* 🚀 ONNX inference code OnnxLLM

## Installation
```sh
# pip install
pip install llmexport
# git install
pip install git+https://github.com/wangzhaode/llm-export@master
# local install
git clone https://github.com/wangzhaode/llm-export && cd llm-export/
pip install .
```

## Usage
1. Clone the LLM project you want to export locally, e.g., chatglm2-6b
```sh
git clone https://huggingface.co/THUDM/chatglm2-6b
# If Hugging Face download is slow, you can use modelscope
git clone https://modelscope.cn/ZhipuAI/chatglm2-6b.git
```
2. Export the model
```sh
# Export chatglm2-6b to ONNX format
llmexport --path ../chatglm2-6b --export onnx
# Export chatglm2-6b to MNN format, with 4-bit quantization and block-wise = 128
llmexport --path ../chatglm2-6b --export mnn --quant_bit 4 --quant_block 128
```
Features
* Supports exporting models to ONNX or MNN format, use `--export onnx` or `--export mnn`
* Supports dialogue testing of the model, use `--test $query` to get the LLM's response
* By default, it uses onnx-slim to optimize ONNX models, skip this step with `--skip_slim`
* Supports exporting after merging LoRA weights, specify LoRA weights directory with `--lora_path`
* Specify quantization bit count with `--quant_bit`; quantization block size with `--quant_block`
* Use `--lm_quant_bit` to specify the quantization bit count for the lm_head layer, if not specified, it uses the `--quant_bit` value
* Supports using your own compiled `MNNConvert`, use `--mnnconvert`

`--test` example
```sh
# Test text input
llmexport --path Qwen2-1.5B-Instruct --test "Hello"
# Test image and text
llmexport --path Qwen2-VL-2B-Instruct  --test "<img>https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-VL/assets/demo.jpeg</img>Describe the content in the image"
```



## Parameters
```
usage: llmexport.py [-h] --path PATH [--type TYPE] [--lora_path LORA_PATH] [--dst_path DST_PATH] [--test TEST] [--export EXPORT]
                    [--skip_slim] [--quant_bit QUANT_BIT] [--quant_block QUANT_BLOCK] [--lm_quant_bit LM_QUANT_BIT]
                    [--mnnconvert MNNCONVERT]

llm_exporter

options:
  -h, --help            show this help message and exit
  --path PATH           path(`str` or `os.PathLike`):
                        Can be either:
                        	- A string, the *model id* of a pretrained model like `THUDM/chatglm-6b`. [TODO]
                        	- A path to a *directory* clone from repo like `../chatglm-6b`.
  --type TYPE           type(`str`, *optional*):
                        	The pretrain llm model type.
  --lora_path LORA_PATH
                        lora path, defaut is `None` mean not apply lora.
  --dst_path DST_PATH   export onnx/mnn model to path, defaut is `./model`.
  --test TEST           test model inference with query `TEST`.
  --export EXPORT       export model to an onnx/mnn model.
  --skip_slim           Whether or not to skip onnx-slim.
  --quant_bit QUANT_BIT
                        mnn quant bit, 4 or 8, default is 4.
  --quant_block QUANT_BLOCK
                        mnn quant block, default is 0 mean channle-wise.
  --lm_quant_bit LM_QUANT_BIT
                        mnn lm_head quant bit, 4 or 8, default is `quant_bit`.
  --mnnconvert MNNCONVERT
                        local mnnconvert path, if invalid, using pymnn.
```

## Supported Models

- llama/llama2/llama3/tinyllama
- qwen/qwen1.5/qwen2/qwen-vl
- baichuan2/phi-2/internlm/yi/deepseek
- chatglm/codegeex/chatglm2/chatglm3
- phi-2/gemma-2
