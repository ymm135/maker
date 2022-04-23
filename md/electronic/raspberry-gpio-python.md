# raspberry-gpio-python 

操作树莓派的GPIO库有很多， [WiringPI](https://github.com/WiringPi/WiringPi), [Pigpio](http://abyz.me.uk/rpi/pigpio/), [Gpiozero](https://gpiozero.readthedocs.io/en/stable/), [RPI.GPIO](https://pypi.org/project/RPi.GPIO/)  

这里主要学习`RPI.GPIO` python 的使用方法  

导入
```python
import RPi.GPIO as GPIO
```

引脚编号声明:
导入 RPi.GPIO 模块后，下一步是确定要使用两种引脚编号方案中的哪一种：

- `GPIO.BOARD`-- 引脚编号遵循接头 P1 上的引脚编号。
- `GPIO.BCM` -- Broadcom 芯片特定的引脚号。这些引脚编号遵循 Raspberry Pi 的 Broadcom 编号系统。  
  
```python
GPIO.setmode(GPIO.BCM)
```

## 输出(Output)
### 数字信号输出  
要将引脚写入高电平或低电平，请使用该 `GPIO.output([pin], [GPIO.LOW, GPIO.HIGH])`函数。例如，如果要将引脚 18 设置为高电平，请编写：
```python
GPIO.output(18, GPIO.HIGH)  
```
写入一个引脚GPIO.HIGH会将其驱动至 3.3V，GPIO.LOW并将其设置为 0V。替代 GPIO.HIGHand GPIO.LOW，您可以使用1, True,0或False来设置 pin 值。  

## PWM（“模拟”）输出  
`analog signal` [ˈænəlɒɡ] (模拟信号)  
Raspberry Pi 上的 PWM 几乎是有限的, 一个，单针可以做到：18（即板针 12）。

要初始化 PWM，请使用 `GPIO.PWM([pin], [frequency])` 函数。为了使其余的脚本编写更容易，您可以将该实例分配给一个变量。然后使用 `pwm.start([duty cycle])`函数设置初始值。例如  

```python
pwm = GPIO.PWM(18, 1000)
pwm.start(50)
```

将我们的 PWM 引脚设置为 1kHz 的频率，并将该输出设置为 50% 的占空比。  

要调整 PWM 输出的值，请使用该 `pwm.ChangeDutyCycle([duty cycle])` 函数。`[duty cycle]` 可以是 0（即 0%/LOW）和 100（即 100%/HIGH）之间的任何值。例如，要将引脚设置为 75%，您可以编写：
```python
pwm.ChangeDutyCycle(75)
```

要关闭该引脚上的 PWM，请使用该 `pwm.stop()` 命令。

够简单！只是不要忘记在将其用于 PWM 之前将其设置为输出。  

## 输入（Input）
如果引脚配置为输入，您可以使用该 `GPIO.input([pin])` 函数读取其值。该input()函数将返回一个True或False指示引脚是高电平还是低电平。您可以使用if语句来测试它，例如
```python
if GPIO.input(17):
    print("Pin 11 is HIGH")
else:
    print("Pin 11 is LOW")
```

将读取引脚 17 并打印它是被读取为高电平还是低电平。  

### 上拉/下拉电阻
还记得 `GPIO.setup()` 我们声明引脚是输入还是输出的函数吗？该函数有一个可选的第三个参数，您可以使用它来设置上拉或下拉电阻。要在引脚上使用上拉电阻，请将 `pull_up_down=GPIO.PUD_UP`  其作为第三个参数添加到GPIO.setup. 或者，如果您需要一个下拉电阻，请改用 `pull_up_down=GPIO.PUD_DOWN`.  

例如，要在 GPIO 17 上使用上拉电阻，请将其写入您的设置：
```python
GPIO.setup(17, GPIO.IN, pull_up_down=GPIO.PUD_UP)
```

如果在第三个值中没有声明任何内容，则两个上拉电阻都将被禁用。  



