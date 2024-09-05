# btc
比特币核心钱包自动输入密码确认
如果每次输入完密码后需要按 **两次确认**，你可以在密码输入后增加一次额外的 `Enter` 按键操作。

我已经更新了代码，在密码输入后按下两次 `Enter` 键：

### 更新后的代码（含双重确认）：

```python
import pyautogui
import pygetwindow as gw
import secrets
import string
import time

def generate_random_password(length=12):
    """生成随机密码，包含大小写字母、数字和特殊字符"""
    characters = string.ascii_letters + string.digits + string.punctuation
    password = ''.join(secrets.choice(characters) for _ in range(length))
    return password

def find_and_activate_bitcoin_wallet():
    """查找并激活比特币核心钱包窗口"""
    windows = gw.getWindowsWithTitle('Bitcoin Core')  # 比特币核心钱包的窗口标题
    if windows:
        wallet_window = windows[0]
        wallet_window.activate()  # 激活窗口
        return True
    else:
        print("Bitcoin Core window not found.")
        return False

def enter_password(password):
    """模拟键盘输入密码并按两次回车键确认"""
    pyautogui.typewrite(password)  # 输入密码
    pyautogui.press('enter')  # 第一次按回车键确认
    time.sleep(0.5)  # 等待0.5秒
    pyautogui.press('enter')  # 第二次按回车键确认

def automate_password_input(interval=30):
    """自动化循环输入密码，间隔指定的秒数"""
    while True:
        password = generate_random_password()  # 生成随机密码
        print(f"Generated Password: {password}")  # 显示生成的密码

        # 查找并激活比特币核心钱包窗口
        if find_and_activate_bitcoin_wallet():
            time.sleep(2)  # 等待2秒，确保窗口激活
            enter_password(password)  # 输入密码并自动确认
            print("Password entered and confirmed.")
        else:
            print("Failed to activate Bitcoin Core window.")

        time.sleep(interval)  # 等待指定的间隔时间后再次执行

if __name__ == "__main__":
    automate_password_input(interval=60)  # 每60秒重复执行一次

```

### 主要改进：

1. **双重确认**：
    - 在 `enter_password()` 函数中，增加了第二次 `pyautogui.press('enter')` 来模拟按下两次回车键。
    - `time.sleep(0.5)` 用于在两次按 `Enter` 键之间等待0.5秒，防止按得太快。
2. **自动化循环**：
    - 程序会继续以 `interval` 为间隔的时间进行密码生成、输入和双重确认的循环，默认每60秒执行一次。

### 使用方法：

1. 启动比特币核心钱包，并确保密码输入框已经弹出。
2. 运行此Python代码，程序会自动每隔指定的时间生成一个随机密码并输入，然后按下两次 `Enter` 键来确认密码输入。

### 注意事项：

- 可以根据实际情况调整 `time.sleep(0.5)` 的时间。如果比特币核心钱包需要更长的时间来处理第一次确认，你可以增加这个时间。
