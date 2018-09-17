# project3
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int *GetCharNum(char *filename, int *TotalNum)
{
	FILE *p;//指向文件的指针
	char buffer[103];//缓冲区，存储读取到的每行的内容
	int len;//缓冲区中实际存储的内容的长度
	int i;//当前读到缓冲区的第i个字符
	char c;//读取到的字符
	int blank = 0;//上个字符是否是空格
	int charNum = 0;//当前行的字符数
	int wordNum = 0;//当前行的单词数
	if((p = fopen(filename,"rb")) == NULL)
	{
		perror(filename);
		return NULL;
	}
	printf("line    words    chars\n");
	while(fgets(buffer,103,p)!=NULL)
	{
		len = strlen(buffer);
		//遍历缓冲区的内容
		for(i=0;i<len;i++)
		{
			c = buffer[i];
			if(c == ' '||c == '\t')//遇到空格
			{
				!blank&&wordNum++;//如果上个字符不是空格，那么单词数加1
				blank = 1;
			}
			else if(c!='\n' && c!='\r')//忽略换行符
			{
				charNum++;//如果既不是换行符也不是空格，字符数加1
				blank = 0;
			}
		}
		!blank && wordNum++;//如果最后一个字符不是空格，那么单词数加1
		blank = 1;//每次换行重置为1
		//一行结束，计算总字符数、总单词数、总行数
		TotalNum[0]++;//总行数
		TotalNum[1] += charNum;//总字符数
		TotalNum[2] += wordNum;//总单词数
		printf("%-10d%-10d%-10d\n", TotalNum[0],wordNum,charNum);
		//置零，重新统计下一行
		charNum = 0;
		wordNum = 0;

	}
	return TotalNum;
}
int main()
{
	char filename[30];
	//TotalNum[0]:总行数  TotalNum[1]:总字符数  TotalNum[2]:总单词数
	int TotalNum[3] = {0,0,0};
	printf("Please input the file name:");
	scanf("%s",filename);
	if(GetCharNum(filename,TotalNum))
	{
		printf("Total:%d lines, %d words, %d chars\n", TotalNum[0],TotalNum[2], TotalNum[1]);
	}
	else
	{
		printf("Error!\n");
	}
	system("pause");
	return 0;
}
