---
title: 'Linux: gpio-keys'
top: false
tags:
  - Linux
date: 2022-06-24 17:02:44
categories: 编程相关
---

`gpio-keys` is part of Linux input subsystem that can be used as key driver which can generate gpio interrupt. Comparing with user-implemented drivers base on GPIO subsystem, `gpio-keys` is much easier to use since it supports features like debounce/autorepeat by default.

<!--more-->

## Device tree bindings

node properties:
`compatible = "gpio-keys"`: Configure node as gpio-keys.
`autorepeat`: Boolean, Enable auto repeat feature of Linux input

subnode properties:
- `linux,code`: Keycode to emit.
- `interrupts-extended` or `interrupts`: Interrupt line for that input.
- `debounce-interval`: Debouncing interval time in milliseconds. If not specified defaults to 5.

> Kernel Documentation: [Device-Tree bindings for input/keyboard/gpio_keys.c keyboard driver](https://www.kernel.org/doc/Documentation/devicetree/bindings/input/gpio-keys.txt)

Examples for STM32MP1:

```dts
user-keys {
	compatible = "gpio-keys";
	#address-cells = <1>;
	#size-cells = <0>;
	autorepeat;
	status = "okay";
	/* gpio needs vdd core in retention for wakeup */
	power-domains = <&pd_core_ret>;

	button@1 {
		label = "PA13";
		linux,code = <BTN_1>;
		interrupts-extended = <&gpioa 13 IRQ_TYPE_EDGE_FALLING>;
		status = "okay";
		wakeup-source;
	};

	button@2 {
		label = "PA14";
		linux,code = <BTN_2>;
		interrupts-extended = <&gpioa 14 IRQ_TYPE_EDGE_FALLING>;
		status = "okay";
		wakeup-source;
	};
};
```

## User space operations

### Check input devices

```bash
cat /proc/bus/input/devices

...

I: Bus=0019 Vendor=0001 Product=0001 Version=0100
N: Name="user-keys"
P: Phys=gpio-keys/input0
S: Sysfs=/devices/platform/user-keys/input/input3
U: Uniq=
H: Handlers=event3 
B: PROP=0
B: EV=100003
B: KEY=6 0 0 0 0 0 0 0 0
```

In outputs we know that `user-keys` are binding with handler `event3`.

### Quick test with `hexdump`

```bash
cat /dev/input/event3 | hexdump
```

Press the button you may see following hex outputs:

```
0000000 42c3 5f67 0306 0002 0001 0102 0001 0000
0000010 42c3 5f67 0306 0002 0000 0000 0000 0000
0000020 42c3 5f67 34cd 0002 0001 0102 0000 0000
0000030 42c3 5f67 34cd 0002 0000 0000 0000 0000
```

### Quick test with C

```c
#include <fcntl.h>
#include <linux/input.h>
#include <stdio.h>
#include <unistd.h>

#define INPUT_EVENT "/dev/input/event3"

int main() {
  int fd = 0;
  struct input_event ev;
  int size = sizeof(ev);

  fd = open(INPUT_EVENT, O_RDONLY);

  while (1) {
    if (read(fd, &ev, size) == size) {
      printf("value is %d, type is %d, code is %d\n", ev.value, ev.type,
             ev.code);
    } else {
      break;
    }
  }

  close(fd);

  return 0;
}
```
