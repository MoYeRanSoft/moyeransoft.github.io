# MoFish
### 为什么选择MoFsih？
||MoFish|其他软件|
| ------------ | ------------ | ------------ |
| 体量 | 轻量，需要什么开什么，快速启动 | 巨量，消耗内存严重，启动速度慢 |
| 启动 | 超快 | 极慢 |
| 功能 | 内置数百种功能，支持无限拓展 | 功能多，但是不能拓展 |
| 效率 | 致力于高效办公，操作简单易用，快速上手 | 功能冗余，难以上手 |
| 风格 | 简介无广告，界面任意摆放 | 广告多，经常有弹窗，界面复杂 |
| 使用 | 组件排列自由组合，需要什么添加什么 | 使用困难，杂乱无章 |
| 界面 | 分离式界面，可以只启动其中任一部分，也可全部启动 | 统一式界面，启动慢，渲染时间长 |
| 美观 | UI自主设计，全平台统一 | 不同平台有着不同平台的装饰器 |
| 安全 | 自研加密算法，点对点安全发送，服务器不储存任何私密文件，软件开源  | 泄露用户信息严重，软件不开源 |
| 价格 | 免费开源，价格亲民 | 不开源，价格昂贵 |


### 无限拓展是什么？
##### 无限拓展是[MoYeRanSoft](https://moyeransoft.netlify.app/ "MoYeRanSoft")自研的一种软件形式，可以自主在软件中添加自己想要的功能，并且功能开发简单，包容性强，并且由于无限拓展是软件开发之初就支持的功能，因此与所有版本(包括测试版)都兼容。
###### 以下为一个拓展功能的源代码示例
```python
# ----- PixivReader.py -----
# * mode = FishModule *
r"""docs:
* ------------------------------------------------------------------------------------------
*	What is MoYeRanSoft?
*		MoYeRanSoft is a chinese company which is good at developing software.
*		A lot of great software were developed by us.
*
*	What is PixivReader?
*		PixivReader can read the picture from pixiv.net.
* ------------------------------------------------------------------------------------------
"""
# ---------------------------------------------------------------------------------------------------- code * UTF-8
# by MoYeRanSoft - MoYeRanQianZhi

import io
import json
from threading import Thread

import requests
from ttkbootstrap import *
from PIL import Image, ImageTk


class MainClass:
    def __init__(
            self,
            master,
            root,
            r18=0,
            size='original',
            *args
    ):
        """
        Parameters:

            master():
                a tkinter window
            r18(int):
                0 or 1
            size(Str):
                size must be str and in ['original','regular','small','thumb','mini']
        """

        self.url = 'https://api.lolicon.app/setu/v2'
        self.master = master
        self.root = root
        self.r18 = r18
        self.size = size
        self.image = None
        self.information = StringVar()
        self.app = Frame(
            master=self.master,
            bootstyle='default'
        )
        self.PixivManager = LabelFrame(
            master=self.app,
            text='PixivManager',
            bootstyle='info'
        )
        self.PixivInformation = Label(
            master=self.PixivManager,
            textvariable=self.information
        )
        self.PixivGetter = Button(
            master=self.PixivManager,
            text='NEW',
            command=self.change
        )

        self.PixivInterface = LabelFrame(
            master=self.app,
            text='PixivRender',
            bootstyle='success'
        )
        self.PixivShower = Label(
            master=self.PixivInterface,
            image=self.image
        )

    def main(self):
        self.app.pack()
        self.PixivManager.pack(
            fill=BOTH,
            side=TOP,
            padx=5,
            pady=5
        )
        self.PixivInformation.pack(
            fill=BOTH,
            side=TOP,
            padx=5,
            pady=5
        )
        self.PixivGetter.pack(
            side=BOTTOM,
            padx=5,
            pady=5
        )
        self.PixivInterface.pack(
            fill=BOTH,
            padx=5,
            pady=5
        )
        self.PixivShower.pack()

    def set(self):
        pass

    def pop(self, master):
        self.master = master

        self.app = Frame(
            master=self.master,
            bootstyle='default'
        )
        self.PixivManager = LabelFrame(
            master=self.app,
            text='PixivManager',
            bootstyle='info'
        )
        self.PixivInformation = Label(
            master=self.PixivManager,
            textvariable=self.information
        )
        self.PixivGetter = Button(
            master=self.PixivManager,
            text='NEW',
            command=self.change
        )

        self.PixivInterface = LabelFrame(
            master=self.app,
            text='PixivRender',
            bootstyle='success'
        )
        self.PixivShower = Label(
            master=self.PixivInterface,
            image=self.image
        )

        self.main()

    def clean(self, master):
        self.master = master

        self.app = Frame(
            master=self.master,
            bootstyle='default'
        )
        self.PixivManager = LabelFrame(
            master=self.app,
            text='PixivManager',
            bootstyle='info'
        )
        self.PixivInformation = Label(
            master=self.PixivManager,
            textvariable=self.information
        )
        self.PixivGetter = Button(
            master=self.PixivManager,
            text='NEW',
            command=self.change
        )

        self.PixivInterface = LabelFrame(
            master=self.app,
            text='PixivRender',
            bootstyle='success'
        )
        self.PixivShower = Label(
            master=self.PixivInterface,
            image=self.image
        )

        self.main()

    def change(self):
        Thread(
            target=self.change_thread,
            daemon=True
        ).start()

    def change_thread(self):
        data = self.get()
        print(data)
        self.information.set(
            f"TITLE:{data['title']}\nAUTHOR:{data['author']}"
        )
        self.image = ImageTk.PhotoImage(
            Image.open(
                io.BytesIO(
                    requests.get(
                        data['url']
                    ).content
                )
            )
        )
        self.PixivShower.config(image=self.image)

    def get(
            self,
            tag=None
    ):
        try:
            if tag is not None and tag != '':
                post_data = {
                    'r18': self.r18,
                    'tag': tag,
                    'size': self.size
                }
            else:
                post_data = {
                    'r18': self.r18,
                    'size': self.size
                }
            response = requests.post(url='https://api.lolicon.app/setu/v2', json=post_data)
            response_json = json.loads(response.text)
            if response_json['error'] == '':
                return {
                    'title': response_json['data'][0]['title'],
                    'author': response_json['data'][0]['author'],
                    'url': response_json['data'][0]['urls'][self.size]
                }
        except Exception as e:
            raise e


if __name__ == '__main__':
    root = Window()
    root.geometry('1000x1000')
    MainClass(root, root, size='regular', r18=1).main()
    root.mainloop()
```
