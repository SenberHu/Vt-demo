使用KmdManager.exe来加载和控制驱动。

0x222000：不触发PG的(S)SSDT HOOK测试。HOOK了NtOpenProcess，并在WIN7X64上HOOK了NtUserCreateWindowEx（其它系统也支持SSSDT HOOK，只是没去硬编码该函数在其他系统上的INDEX）。
测试过程：第一次CALL该控制码开启HOOK，第二次CALL该控制码关闭HOOK，以此类推。
　　　　　测试完毕后，请确认HOOK状态为“关闭”。以防干扰其它测试。

0x222008：利用调试寄存器实现的R3无痕HOOK。
测试过程：先打开vt_drx_hook.exe，然后CALL该控制码，然后按下回车让程序继续执行。可发现没调用MessageBoxA而是跳转到了代理函数（MyMessageBoxA）。
　　　　　如果没有CALL该控制码，就继续，则程序崩溃。
　　　　　如果没有CALL该控制码，但给进程附加调试器，就继续，会弹出对话框。

0x22200C：利用TF标志位实现的R3无痕HOOK。
测试过程：先打开vt_tf_hook.exe，然后CALL该控制码，等待10秒后，可发现没调用MessageBoxA而是跳转到了代理函数（MyMessageBoxA）。
　　　　　如果没有CALL该控制码，会弹出对话框。

0x222014：利用TF标志位实现的无限执行硬断。
测试过程：先打开vt_hwbp.exe，然后CALL该控制码，然后用调试器附加这个EXE，然后按下回车让程序继续执行。
　　　　　输入1～6来使程序调用相应的API，这时调试器会断下，按下F5可以继续执行。
　　　　　测试完毕后，再次CALL该控制码，解除“无限执行硬断”的测试状态。