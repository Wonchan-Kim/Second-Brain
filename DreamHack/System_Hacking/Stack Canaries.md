## Why Stack Canary was introduced

![](https://dreamhack-lecture.s3.amazonaws.com/media/2e6f80229f8350969409af607b251cc2cd7f3fe4c82484f5bc52b466dbffe72d.gif)

Exploiting stack buffer overflow allowed to modify the return address of the stack. Stack canary was the protection method against stack buffer overflow. 

As shown in the gif file above, when the attacker tries to exploit the return address (rbp + 4 or rbp + 8 depending on the x32/x64 system), the attacker who doesn’t know the value stored in the canary will modify the value stored. 

> [!note] Canary name origin
> From the bird name yes, Canaries react more sensitive to the carbon monoxide than human, allowing miners to avoid the risk of being addicted to it. 

``` canary.c

```