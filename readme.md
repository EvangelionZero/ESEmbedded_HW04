HW03
===
#1.實驗題目

修改reg.h標頭檔裡面的macro function,並建立read_bit讀取STM3207F4的input
修改blink.c,使在reset後按下user_button藍色LED才閃爍

#2.實驗步驟

*1.打開reg.h,依據上課講義與使用者手冊,建立新的macro function,包含rcc,ldr reg等相關函式
*2.查閱手冊後,發現user button所使用到的位置是PA0
*3.建立read_bit函式,其方法為addr與uint_1"<<"bits進行and,透過這個方法可以過濾掉其他沒有要用的input
```
	#define READ_BIT(addr, bit)  (REG(addr) &= UINT32_1 << (bit)) 
	#define GPIOx_IDR_OFFSET 0x10
	#define IDRy_BIT(y) (y)
```
*3.打開blink.c檔，按照上課講義與使用者手冊,先以rcc初始化PortA,再以reg設定PA的型態,使之成為input
```
	SET_BIT(RCC_BASE + RCC_AHB1ENR_OFFSET, GPIO_EN_BIT(GPIO_PORTA));

	//MODER user button = 00 => General purpose input mode  A
	CLEAR_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_MODER_OFFSET, MODERy_1_BIT(0));
	CLEAR_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_MODER_OFFSET, MODERy_0_BIT(0));

```

*4.在main裡面加上進入`while`的條件,並在外圍再加上一個`while`以確保按按鈕之前不會結束程式
```
	int a;
		while(1)
		{
			a=READ_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_IDR_OFFSET,IDRy_BIT(0));
			if(a)
			{ ......（blink）
```

* 5.為`STM3207F4`上電,在作業資料夾內打開終端機,依序輸入`make clean`,`make`,`make flash`,並看是否有完成題目要求

# 3.結果與討論
* 1.本次我學到的教訓是只要漏掉一個步驟，例如忘記初始化,按鈕就是一個沒有用的東西


- [] **If you volunteer to give the presentation next week, check this.**

--------------------

**★★★ Please take your note here ★★★**
