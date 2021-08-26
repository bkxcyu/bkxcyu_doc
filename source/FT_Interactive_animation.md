任何一个工科生都不会对傅利叶变换感到陌生:傅利叶变换可以将一个信号从时域转换到复频域,这样就能愉快地在复频域分析信号啦.虽然在用的时候司空见惯,但在某一些时刻我还是会忍不住感叹--傅利叶变换真是太妙了!



其实,从在课堂上学到傅利叶变换后的很长一段时间里我都还没有领会到其妙处,直到我看到了这个视频: 

```{image} img/ft_video.png
:alt: 形象展示傅里叶变换
:class: bg-primary
:width: 400px
:align: center
:target: https://www.bilibili.com/video/BV1pW411J7s8
```



这个视频通过数学动画将傅利叶变换形象地展示了出来.受这个视频的启发,我也写了一个交互式的小程序来体会傅利叶变换的精妙之处.

```{image} img/F.gif
:alt: ft1
:width: 400px
:align: center
```


```{admonition} 注意
:class: note
要体验交互功能,您需要{download}`下载.ipynb文件<src/FT_code.ipynb>`后在本地运行.如果您不熟悉jupyter notebook,请看[这里](https://jupyter.org/)
```



---

# 傅利叶变换: 以$f(t) = sin(2\pi t)$​为例

$$\mathscr{F}[f(t)] = \int^{+\infty}_{-\infty} f(t) e^{-i\omega t}dt  $$

傅利叶变换可以分为$f(t)$, $e^{-i\omega t}$和积分这三个部分来看. 下面就以$f(t) = sin(2\pi t)$​为例来可视化一下傅利叶变换的过程.

## 1. 画出$f(t)=sin(2 \pi t)$​图像

```python
%matplotlib inline
from ipywidgets import interactive
import matplotlib.pyplot as plt
from matplotlib.widgets import TextBox
import numpy as np
plt.rcParams['figure.figsize'] = (10.0, 10.0)

def set_center_axis(ax):
    ax.spines['right'].set_color('none')
    ax.spines['top'].set_color('none')
    ax.spines['bottom'].set_position(('data', 0))
    ax.spines['left'].set_position(('data', 0))

t = np.linspace(-2, 2, num=1000)
f = np.sin(2*np.pi*t)
plt.plot(t,f)
set_center_axis(plt.gca())

```

```{image} img/sin.png
:alt: sin
:width: 400px
:align: center
```

## 2. $e^{-i\omega t}$​​长什么样子?

您可以通过下面的交互来体会$\omega$和$t$的含义.

```python
def F(omega,t):
    z = np.exp(-1j*omega*t)
    print(z)
    plt.arrow(0,0,z.real,z.imag,head_width=0.05, head_length=0.1)
    plt.xlim(-1.1,1.1)
    plt.ylim(-1.1,1.1)
    set_center_axis(plt.gca())

interactive_plot = interactive(F,omega=(-2*np.pi, 2*np.pi, 0.01),t=(0, 2*np.pi, 0.1))
output = interactive_plot.children[-1]
output.layout.height = '600px'

interactive_plot
```

```{image} img/E.gif
:alt: E
:width: 400px
:align: center
```


## 3. $sin(2\pi t)e^{-2\pi i \omega t}$ 和 $\left| \mathscr{F}[sin(2\pi t)] \right|$

```python
ft_line = np.empty(shape =(1,2))
def F(freq):
    global ft_line
    fig, (ax1, ax2) = plt.subplots(2, 1)
    t = np.linspace(0, np.pi, num=1000)
    ft = np.exp(2*np.pi*freq*1j*t) * np.sin(2*np.pi*t)
    Amp = abs(ft.sum())
    if ft_line.shape ==(1,2):
        ft_line = np.array([[freq,Amp]])
    ft_line = np.append(ft_line,np.array([[freq,Amp]]),axis=0)
    ax1.plot(ft.real,ft.imag)
    set_center_axis(ax1)
    ax1.set_xlabel('real',position=(1,0),fontsize = 14)
    ax1.set_ylabel('imag',position=(0,1),fontsize = 14)
    ax1.set_aspect(1)
    textstr = '\n'.join((r'$sin(2\pi t)e^{-2\pi it}\cdot e^{%.2f}$' % (freq, ),))
    ax1.text(0.05,1.1,textstr,fontsize = 15)
    ax2.scatter(ft_line[:,0],ft_line[:,1])
    ax2.set_xlabel('$freq$',fontsize=14)
    textstr2 = '\n'.join((r'$ A = \left|\int^{+\infty}_{-\infty}sin(2\pi t)e^{-2\pi it}\cdot e^{%.2f} dt\right|$' % (freq, ),))
    ax2.set_ylabel(textstr2,fontsize = 14)
    
interactive_plot = interactive(F,freq=(-2, 2, 0.01))
output = interactive_plot.children[-1]
output.layout.height = '1000px'

interactive_plot
```

```{image} img/F.gif
:alt: F
:width: 400px
:align: center
```
