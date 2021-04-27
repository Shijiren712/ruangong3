#include<iostream>
#include<ctime>
#include<stdio.h>
#include<stdlib.h>
#define error -1
#define ok 1
 struct windows
{
   int num;//办理的业务为多少号
   int minute;//当前办理业务所需时间
   int minute1,minute2;//1为vip总时间，2为非vip总时间
};
 int flag[20]={0};//用于vip窗口判断是否为会员的标志
 int mark[20]={0};
 int value[30]={0};//用于vip窗口记录每个用户（不分是否为vip）的时间
 int  x[8]={7,6,6,8,8,5,8,6};
 int a[4]={0};
 int W=0;
 int F=0;
 int cnt=0;
 int cnt1=0;
 int num123=0;//记录4窗口是否有人进
 struct windows window[4];

typedef struct
{
	int *base;//业务时间
	int front;
	int rear;
}SqQueue;
void InitQueue(SqQueue &Q)
{
    Q.base=(int *)malloc(100*sizeof(int));
	Q.front=Q.rear=0;
	return ;
}
void EnQueue(SqQueue &Q,int e)
{
      if((Q.rear+1)%100==Q.front)
	  {
	     return ;
	  }
	  Q.base[Q.rear]=e;
	  Q.rear=(Q.rear+1)%100;
	  return ;
}
int DeQueue(SqQueue &Q/*,int &e*/)
{
      if(Q.front==Q.rear)
	  {
	      return error;
	  }
	  /*e=Q.base[Q.front];*/
	  Q.front=(Q.front+1)%100;
	  return Q.front;
}
SqQueue Q[3];
int choose1()//排队窗口
{
     int num1=window[0].minute;
	 int num2=window[1].minute;
	 int num3=window[2].minute;
	 int num4=window[3].minute;
	 if(num1<=num2&&num1<=num3&&num1<=num4)
	 {
	    return 1;
	 }
	 if(num2<num1&&num2<=num3&&num2<=num4)
	 {
	   return 2;
	 }
	 if(num3<num1&&num3<num2&&num3<=num4)
	 {
	    return 3;
	 }
	 if(num4<num1&&num4<num2&&num4<num3)
	 {
	   return 4;
	 }
}
int choosee1()//排队窗口
{
   int num1=window[0].minute;
	 int num2=window[1].minute;
	 int num3=window[2].minute;
	 SqQueue q1;
	 q1=Q[3];
	 if(q1.base[q1.front]<0)
	 {
	    q1.base[q1.front]=0;
	 }
	 int num4=q1.base[q1.front]+window[3].minute1;
	 if(num1<=num2&&num1<=num3&&num1<=num4)
	 {
	    return 1;
	 }
	 if(num2<num1&&num2<=num3&&num2<=num4)
	 {
	   return 2;
	 }
	 if(num3<num1&&num3<num2&&num3<=num4)
	 {
	    return 3;
	 }
	 if(num4<num1&&num4<num2&&num4<num3)
	 {
	   return 4;
	 }
}
int choose2(int a1)//该排队窗口的排队号
{
    return window[a1-1].num++;
}
void operate1(float secs)
{
	 int sec=secs/1;
     int num0=window[0].minute-sec;
	 int num1=window[1].minute-sec;
	 int num2=window[2].minute-sec;
	 int num3=window[3].minute-sec;
	 SqQueue q2;
	 q2=Q[3];
	 if(num0<=0)
	 {
	    window[0].minute=0;
	 }
	 else
	 {
	    window[0].minute=num0;
	 }
	 if(num1<=0)
	 {
	    window[1].minute=0;
	 }
	 else
	 {
	    window[1].minute=num1;
	 }
	 if(num2<=0)
	 {
	    window[2].minute=0;
	 }
	 else
	 {
	    window[2].minute=num2;
	 }
	 if(num3<=0)
	 {
	    window[3].minute=0;
	 }
	 else
	 {
	    window[3].minute=num3;
	 }
	 if(num3<=0)
	 {
	   window[3].minute1=0;
	 }  
	 else//num3>0
	 {
		if(flag[q2.front]==1)
		{
		  while(sec>=0&&flag[q2.front]==1)
		  {
		      if(q2.base[q2.front]>sec)
			  {
			      window[3].minute1=window[3].minute1-sec;
				  q2.base[q2.front]=q2.base[q2.front]-sec;
				  break;
			  }
			  else
			  {
			     sec=sec-q2.base[q2.front];
				 window[3].minute1=window[3].minute1-q2.base[q2.front];
				 q2.front++;
			  }
		  }
		}
		else
		{
		    if(q2.base[q2.front]<sec)
			{
			    sec=sec-q2.base[q2.front];
				q2.front++;
				while(sec>0&&flag[q2.front]==1)
				{
					if(q2.base[q2.front]>sec)
					{
						window[3].minute1=window[3].minute1-sec;
						q2.base[q2.front]=q2.base[q2.front]-sec;
						break;
					}
					else
					{
						sec=sec-q2.base[q2.front];
						window[3].minute1=window[3].minute1-q2.base[q2.front];
						q2.front++;
					}
				}
			}
			else
			{
			    //第一个人又不是会员，他的时间足够减sec，可不考虑
			}
		}
     }
}
void rank1(int number1)
{
	SqQueue q3;
	q3=Q[3];
	int a2=q3.front;
   for(int i=F;i<=a2;i--)
   {
      value[i+1]=value[i];
	  flag[i+1]=flag[i];
   }
   value[a2]=x[number1-1];
   flag[a2]=1;
   F++;
}
void rank2(int number1)
{
    SqQueue q4;
	q4=Q[3];
	int a2=q4.front;
   for(int i=F;i<=(a2+1);i--)
   {
      value[i+1]=value[i];
	  flag[i+1]=flag[i];
   }
   value[a2+1]=x[number1-1];
   flag[a2+1]=1;
   F++;
}
int rank3(int number1)
{
    SqQueue q5;
	q5=Q[3];
	int a2=q5.front;
	while(flag[a2]==1)
	{
	     a2++;
	}
   for(int i=F;i<=a2;i--)
   {
      value[i+1]=value[i];
	  flag[i+1]=flag[i];
   }
   value[a2+1]=x[number1-1];
   flag[a2+1]=1;
   F++;
   return a2;
}
int rank4(int number1)
{
    SqQueue q6;
	q6=Q[3];
	int a2=q6.front;
	a2++;
	while(flag[a2]==1)
	{
	    a2++; 
	}
   for(int i=F;i<=a2;i--)
   {
      value[i+1]=value[i];
	  flag[i+1]=flag[i];
   }
   value[a2+1]=x[number1-1];
   flag[a2+1]=1;
   F++;
   return a2;
}
int main()
{
  int k=0;//第几个标志为1
  F=0;
  InitQueue(Q[0]);
  InitQueue(Q[1]);
  InitQueue(Q[2]);
  InitQueue(Q[3]);
  /*SqQueue p;*/
  window[0].num=0;
  window[1].num=0;
  window[2].num=0;
  window[3].num=0;
  window[0].minute=0;
  window[1].minute=0;
  window[2].minute=0;
  window[3].minute=0;
  window[3].minute1=0;
  window[3].minute2=0;
  using namespace std;
  float time0=0;
  float secs;
  int a1=0,a2=0;//a1为窗口号，a2为排队号
  int b1=1,b2=1;//当前
  for(;time0<8*60;time0+=secs)
  {
	cnt++;
	printf("       -------------------------------- \n");
	printf("       |       银行排号系统            |\n");
	printf("       |        1.开通账户             |\n");
	printf("       |        2.注销账户             |\n");
	printf("       |        3.取钱业务             |\n");
	printf("       |        4.存钱业务             |\n");
	printf("       |        5.同行转款业务         |\n");
	printf("       |        6.跨行转款业务         |\n");
	printf("       |        7.贷款业务             |\n");
	printf("       |        8.更改账户信息         |\n");
	printf("       |尊敬的用户，请选择办理的业务： |\n");
	
	int number1,number2;//随机选择的业务序号，随机会员数
	srand(time(NULL));
	number1= rand()%8+1;
	printf("       |             %d                 |\n",number1);
	printf("       |        是否为VIP会员？        |\n");
	srand(time(NULL));
	number2=rand()%8+1;
	if(number2==1||number2==5)
	{printf("是\n");
	       
		   cnt1++;//记录会员数
		   a1=choosee1();
		   /*flag[k++]=1;*/
		   k++;
	       EnQueue(Q[a1-1],x[number1-1]);//业务时间进入队列中
		   if(a1==4&&num123==0)
		   {
				num123++;
				//在第四个窗口，第一个人无论是否为会员处理
				window[a1-1].minute1=window[a1-1].minute1+x[number1-1];
				value[F++]=x[number1-1];
				a2=window[a1-1].num++;
				flag[0]=1;
				printf("当前正在办理排队号为 %d-%d\n",a1,a2+1);//b1 b2当前排队号
				printf("您的排队号为 %d-%d\n",a1,a2+1);
				printf("尊敬的会员,您现在可以去%d号窗户办理业务\n\n\n",a1);

		   }
			 if(a1==4&&num123!=0)
			 {
				 SqQueue p;
                  p=Q[3];
			   if(window[a1-1].minute1==0)//该会员前方没有会员等待
			   {
				  a2=window[a1-1].num++;
			      if(p.base[--p.front]==0&&mark[++p.front]==0)//当前无人
				  {
					 printf("当前正在办理排队号为 %d-%d\n",a1,a[a1-1]+1);//b1 b2当前排队号
					 printf("您的排队号为 %d-%d\n",a1,a2+1);
	                 printf("尊敬的会员，您将享受会员优先服务\n");
					 printf("您现在可以去%d号窗户办理业务\n\n\n",a1);
					 rank1(number1);
				  }
				  else
				  {
					 printf("当前正在办理排队号为 %d-%d\n",a1,p.front+1);
					 printf("您的排队号为 %d-%d\n",a1,a2+1);
					 printf("尊敬的会员，%d-%d号办理结束将为您优先办理业务",a1,p.front+1);
					/* printf("预计排队时长：%d分钟\n",Q[3].base[p.front]);*/
					 printf("您现在可以去%d号窗口等待办理业务\n\n\n",a1);	
					 rank2(number1);
				  }
			   }
			   else//前方有会员等待
			   {
			   	  a2=window[a1-1].num++;
			      if(p.base[--p.front]==0&&mark[++p.front]==0)//当前无人
				  {
					 printf("当前正在办理排队号为 %d-%d\n",a1,p.front+1);//b1 b2当前排队号
					 printf("您的排队号为 %d-%d\n",a1,rank3(number1)+1);
					 printf("尊敬的会员，您将享受会员优先服务\n");
					 printf("您现在可以去%d号等待窗户办理业务\n\n\n",a1);
				  }
				  else
				  {
					 printf("当前正在办理排队号为 %d-%d\n",a1,p.front+1);
					 printf("您的排队号为 %d-%d\n",a1,rank4(number1)+1);
					 printf("尊敬的会员，您将享受会员优先服务\n");
					 /*printf("尊敬的会员，%d-%d号办理结束将为您优先办理业务",a1,p.front+1);*/
					 printf("预计排队时长：%d分钟\n",Q[3].base[p.front]);
					 printf("您现在可以去%d号窗口等待办理业务\n\n\n",a1);	
				  }
			   }
			   window[a1-1].minute1=window[a1-1].minute1+x[number1-1];//该窗口vip 的排队时间
		   }
	     if(a1!=4)
	   {
		    a2=window[a1-1].num++;
			printf("当前正在办理排队号为 %d-%d\n",a1,a[a1-1]+1);//b1 b2当前排队号
			printf("您的排队号为 %d-%d\n",a1,a2+1);
			printf("预计排队时长：%d分钟\n",window[a1-1].minute);
			if(window[a1-1].minute==0)
			{
				printf("您现在可以去%d号窗户办理业务\n\n\n",a1);
			}
			else
			{
				printf("您可以去%d号窗户等待办理业务\n\n\n",a1);
		}
	  }

	window[a1-1].minute=window[a1-1].minute+x[number1-1];//该窗口加完此人的排队时间
 	srand(time(NULL));
	secs= rand()%5+1;
	/*cout<< "Enter the delay time , in  second : \n" ;*/
	printf("预计下一位用户即将在%0.f分钟后到达",secs);
	//cout<<secs;//延时时间(下一位用户进来的时间)
	clock_t delay = secs * CLOCKS_PER_SEC;
	cout << "\nstaring \a\n";
	cout<<secs;
	cout<<"分钟";
	clock_t start = clock();
	while(clock()-start<delay)
	;
	cout << "\n下一位用户到达银行 \a\n";
	operate1(secs);
	SqQueue q;
      for(int j=0;j<4;j++)
	  {
		q=Q[j];
		int y=Q[j].base[q.front];
		if(y<=0)
		{
			y=0;
		}
		int secss=secs;
		while(secss>=y&&y>0)
		{ 
		  secss=secss-y;
		  a[j]=DeQueue(Q[j]);
		  q=Q[j];
		  y=Q[j].base[q.front];
		}
		if(secss<y)
		{
		   Q[j].base[q.front]=Q[j].base[q.front]-secss;
		}
      }
	}
	else
	{  
		printf("       |             否                |\n");
		printf("        -------------------------------- \n");
  		a1=choose1();
		a2=choose2(a1);
		EnQueue(Q[a1-1],x[number1-1]);
		if(a1==4&&num123==0)
		{
			    num123++;
			    //在第四个窗口，第一个人无论是否为会员处理
				//SqQueue ap;
				//ap=Q[3];
				window[a1-1].minute1=window[a1-1].minute1+x[number1-1];
				value[F++]=x[number1-1];
				flag[0]=1;
				printf("当前正在办理排队号为 %d-%d\n",a1,a2+1);//b1 b2当前排队号
				printf("您的排队号为 %d-%d\n",a1,a2+1);
				printf("您现在可以去%d号窗户办理业务\n\n\n",a1);
		}
		else
		{
			printf("当前正在办理排队号为 %d-%d\n",a1,a[a1-1]+1);//排队号有问题，窗口无问题
			printf("您的排队号为 %d-%d\n",a1,a2+1);
			if(a1!=4)
			{
			   printf("预计排队时长：%d分钟\n",window[a1-1].minute);
			}
			if(a1==4)
			{
				num123++;
				flag[k++]=0;
				value[F++]=x[number1-1];
				if(window[a1-1].minute!=0)
				{
       			  printf("排队时长至少为：%d分钟，该窗口为会员优享窗口，如有vip顾客，将享受优先处理\n",window[a1-1].minute);
				}
			}
			if(window[a1-1].minute==0)
			{
				printf("您现在可以去%d号窗户办理业务\n\n\n",a1);
			}
			else
			{
				printf("您可以去%d号窗户等待办理业务\n\n\n",a1);
			}
			window[a1-1].minute=window[a1-1].minute+x[number1-1];//该窗口加完此人的排队时间
		}
 	srand(time(NULL));
	secs= rand()%3+1;
	printf("        -------------------------------------\n");
	printf("        预计下一位用户即将在%0.f分钟后到达   |",secs);
	clock_t delay = secs * CLOCKS_PER_SEC;
	cout << "\n             staring \a\n";
	cout<<secs;
	cout<<"分钟";
	clock_t start = clock();
	while(clock()-start<delay)
	;
	cout << "\n下一位用户到达银行 \a\n";
	operate1(secs);
	SqQueue q;
    for(int j=0;j<4;j++)
	  {
		q=Q[j];
		int y=Q[j].base[q.front];
		if(y<=0)
		{
			y=0;
		}
		int secss=secs;
		while(secss>=y&&y>0)
		{ 
		  secss=secss-y;
		  a[j]=DeQueue(Q[j]);
		  q=Q[j];
		  y=Q[j].base[q.front];
		}
		if(secss<y)
		{
		   Q[j].base[q.front]=Q[j].base[q.front]-secss;
		   mark[q.front]=1;
		}
	  }
    }
  }
  printf("今天银行一共为%d名顾客办理业务",cnt);
  return 0;
 }
