 import random
import time
import math
import platform

# 跨平台聲音處理
def play_beep():
    try:
        if platform.system() == "Windows":
            import winsound
            winsound.Beep(1000, 500)
        else:
            import os
            os.system('play -nq -t alsa synth 0.5 sine 1000')  # Linux/macOS 可用 sox
    except Exception as e:
        print(f"(無法播放聲音: {e})")

# 模擬地震數據
def read_accelerometer():
    if random.random() < 0.95:
        return random.uniform(-0.1, 0.1), random.uniform(-0.1, 0.1), random.uniform(0.9, 1.1)
    else:
        return random.uniform(-3, 3), random.uniform(-3, 3), random.uniform(-3, 3)

def vector_magnitude(x, y, z):
    return math.sqrt(x**2 + y**2 + z**2)

def trigger_alarm():
    print("⚠️⚠️⚠️ 地震警報！請立即避難 ⚠️⚠️⚠️")
    for _ in range(3):
        play_beep()
        time.sleep(0.2)

def main():
    threshold = 1.5
    print("🛰️ 地震偵測器已啟動... (按 Ctrl+C 結束)")
    try:
        while True:
            x, y, z = read_accelerometer()
            magnitude = vector_magnitude(x, y, z)
            print(f"目前加速度值: {magnitude:.2f} g")
            if magnitude > threshold:
                trigger_alarm()
            else:
                print("✅ 正常")
            time.sleep(1)
    except KeyboardInterrupt:
        print("\n已停止監測。")

if __name__ == "__main__":
    main()