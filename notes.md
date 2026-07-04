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

# 3. 理解任务目标
目前可以拿到1000条样本，其中700条是分类场景，300条是多轮对话意图识别场景，不过可以一起训练。

参赛时期选手还可以拿到10000条无标签数据，选手可以基于这些数据生成伪标签扩充训练集。模型训好之后，也是在这10000条上推理，提交推理结果到平台自动评估。进入复赛的选手还可以拿到复赛提供的另外10000条无标签数据。


# 4. 数据特征理解
总标签：
```
total labels:46种-{'气泡', '是否好用', '是否会生锈', '退款页面', '发货数量', '包装区别', '养护方法', '购物车页面', '活动页面', '物流页面-物流跟踪页面', '信号情况', '评论区截图页面', '适用季节', '物流页面-物流列表页面', '反馈用后症状', '商品分类选项', '商品详情页截图', '退货页面', '控制方式', '是否易褪色', '上市时间', '反馈密封性不好', '换货页面', '其他类别图片', '用法用量', '单品推荐', '商品材质', '商品规格', '能否调光', '下单过程中出现异常（显示购买失败浮窗）', '商品头图', '店铺页面', '投诉举报页面', '支付页面', '功效功能', '实物拍摄(含售后)', '订单详情页面', '账单/账户页面', '何时上货', '优惠券领取页面', '套装推荐', '排水方式', '物流页面-物流异常页面', '版本款型区别', '外部APP截图', '平台介入页面'},  {'气泡', '是否好用', '是否会生锈', '退款页面', '发货数量', '包装区别', '养护方法', '购物车页面', '活动页面', '物流页面-物流跟踪页面', '信号情况', '评论区截图页面', '适用季节', '物流页面-物流列表页面', '反馈用后症状', '商品分类选项', '商品详情页截图', '退货页面', '控制方式', '是否易褪色', '上市时间', '反馈密封性不好', '换货页面', '其他类别图片', '用法用量', '单品推荐', '商品材质', '商品规格', '能否调光', '下单过程中出现异常（显示购买失败浮窗）', '商品头图', '店铺页面', '投诉举报页面', '支付页面', '功效功能', '实物拍摄(含售后)', '订单详情页面', '账单/账户页面', '何时上货', '优惠券领取页面', '套装推荐', '排水方式', '物流页面-物流异常页面', '版本款型区别', '外部APP截图', '平台介入页面'}

```
分类标签：
<img width="1779" height="690" alt="image" src="https://github.com/user-attachments/assets/784715ab-bb12-4856-a08f-7db4d72fb250" />

意图识别标签：
<img width="1790" height="690" alt="image" src="https://github.com/user-attachments/assets/6297146d-9292-4e6e-8b49-ae49c1283891" />


# 5. 划分数据集9:1 跑基线训练和测试




