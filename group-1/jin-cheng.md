# 进程

### 记录

现在脑中想到的就是进程是任务调度的一个最小单位，这里内核线程应该也算是任务调度的单位。然后进程控制块就是进程的唯一实体，进程的相关信息全都在这个进程控制块的结构体当中。进程控制块放在就绪队列中（阻塞队列等），任务调度在就绪队列中根据算法（简单的调度算法就是根据进程的优先级）选择一个进程控制块中优先级最高的从就绪队列中取出将其变成运行态，这个进程控制块将享受时间片，然后根据调度算法将这个放入到阻塞等进程状态队列等待下一次的调度；&#x20;

用户级的多线程其实就是进程享受的CPU时间片然后在进程内部进行分配给不同的线程，就会出现线程是运行态，所在的进程处在阻塞态，但是处在运行态的线程实际上是不工作的。操作系统只关心进程所以说进程是任务调度的最小单位。如果是内核级线程的话那么操作系统是可以知道这个线程的存在的。

### FreeRTOS源码理解进程

```
void vTaskStartScheduler( void )
{
    /* 手动指定第一个运行的任务 */
    pxCurrentTCB = &Task1TCB;
    
    /* 启动调度器 */
    if( xPortStartScheduler() != pdFALSE )
    {
        /* 调度器启动成功，则不会返回，即不会来到这里 */
    }
}
BaseType_t xPortStartScheduler( void )
{
    /* 配置PendSV 和 SysTick 的中断优先级为最低 */
	portNVIC_SYSPRI2_REG |= portNVIC_PENDSV_PRI;
	portNVIC_SYSPRI2_REG |= portNVIC_SYSTICK_PRI;

	/* 启动第一个任务，不再返回 */
	prvStartFirstTask();

	/* 不应该运行到这里 */
	return 0;
}
__asm void prvStartFirstTask( void )
 {
	PRESERVE8

	/* 在Cortex-M中，0xE000ED08是SCB_VTOR这个寄存器的地址，
       里面存放的是向量表的起始地址，即MSP的地址 */
	ldr r0, =0xE000ED08
	ldr r0, [r0]
	ldr r0, [r0]

	/* 设置主堆栈指针msp的值 */
	msr msp, r0
    
	/* 使能全局中断 */
	cpsie i
	cpsie f
	dsb
	isb
	
    /* 调用SVC去启动第一个任务 */
	svc 0  
	nop
	nop
}

__asm void xPortPendSVHandler( void )
{
	extern pxCurrentTCB;
	extern vTaskSwitchContext;

	PRESERVE8

    /* 当进入PendSVC Handler时，上一个任务运行的环境即：
       xPSR，PC（任务入口地址），R14，R12，R3，R2，R1，R0（任务的形参）
       这些CPU寄存器的值会自动保存到任务的栈中，剩下的r4~r11需要手动保存 */
    /* 获取任务栈指针到r0 */
	mrs r0, psp
	isb

	ldr	r3, =pxCurrentTCB		/* 加载pxCurrentTCB的地址到r3 */
	ldr	r2, [r3]                /* 加载pxCurrentTCB到r2 */

	stmdb r0!, {r4-r11}			/* 将CPU寄存器r4~r11的值存储到r0指向的地址 */
	str r0, [r2]                /* 将任务栈的新的栈顶指针存储到当前任务TCB的第一个成员，即栈顶指针 */				
                               

	stmdb sp!, {r3, r14}        /* 将R3和R14临时压入堆栈，因为即将调用函数vTaskSwitchContext,
                                  调用函数时,返回地址自动保存到R14中,所以一旦调用发生,R14的值会被覆盖,因此需要入栈保护;
                                  R3保存的当前激活的任务TCB指针(pxCurrentTCB)地址,函数调用后会用到,因此也要入栈保护 */
	mov r0, #configMAX_SYSCALL_INTERRUPT_PRIORITY    /* 进入临界段 */
	msr basepri, r0
	dsb
	isb
	bl vTaskSwitchContext       /* 调用函数vTaskSwitchContext，寻找新的任务运行,通过使变量pxCurrentTCB指向新的任务来实现任务切换 */ 
	mov r0, #0                  /* 退出临界段 */
	msr basepri, r0
	ldmia sp!, {r3, r14}        /* 恢复r3和r14 */

	ldr r1, [r3]
	ldr r0, [r1] 				/* 当前激活的任务TCB第一项保存了任务堆栈的栈顶,现在栈顶值存入R0*/
	ldmia r0!, {r4-r11}			/* 出栈 */
	msr psp, r0
	isb
	bx r14                      /* 异常发生时,R14中保存异常返回标志,包括返回后进入线程模式还是处理器模式、
                                   使用PSP堆栈指针还是MSP堆栈指针，当调用 bx r14指令后，硬件会知道要从异常返回，
                                   然后出栈，这个时候堆栈指针PSP已经指向了新任务堆栈的正确位置，
                                   当新任务的运行地址被出栈到PC寄存器后，新的任务也会被执行。*/
	nop
}
```

![](../.gitbook/assets/IMG\_4461.JPG)





