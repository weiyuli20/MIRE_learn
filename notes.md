# 1. 数据集下载
目前无法从官网下载数据集，可以从modelscope 下载：https://www.modelscope.cn/datasets/smau0441/www2025-train

```
pip install modelscope
modelscope download --dataset smau0441/www2025-train
```

# 2. 下载安装Llamafactory
```
git clone --depth 1 https://github.com/hiyouga/LlamaFactory.git
cd LlamaFactory
pip install -e .
pip install -r requirements/metrics.txt

llamafactory-cli webui

```

解决autodl无法设置gradio共享链接的问题：

修改LlamaFactory/src/llamafactory/webui/interface.py。 gradio_share 设置为 True

```
def run_web_ui() -> None:
    gradio_ipv6 = is_env_enabled("GRADIO_IPV6")
    # gradio_share = is_env_enabled("GRADIO_SHARE")
    gradio_share = True
    server_name = os.getenv("GRADIO_SERVER_NAME", "[::]" if gradio_ipv6 else "0.0.0.0")
    print("Visit http://ip:port for Web UI, e.g., http://127.0.0.1:7860")
    fix_proxy(ipv6_enabled=gradio_ipv6)
    create_ui().queue().launch(share=gradio_share, server_name=server_name, inbrowser=True)
```
1. Download this file: https://cdn-media.huggingface.co/frpc-gradio-0.3/frpc_linux_amd64
2. Rename the downloaded file to: frpc_linux_amd64_v0.3
3. Move the file to this location: /root/.cache/huggingface/gradio/frpc
4. chmod +x /root/.cache/huggingface/gradio/frpc/frpc_linux_amd64_v0.3
