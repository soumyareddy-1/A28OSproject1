//question 2 code where multi-threading is used
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>
void *averagefunction(void *);
void *maximumfunction(void *);
void *minimumfunction(void *);
int no_of_numbers,average=0,maximum=0,minimum;
void *p;
int main()
{
	int loop_var;
	pthread_t thread_for_average,thread_for_maximum,thread_for_minimum;
	printf("enter no of numbers");
	scanf("%d",&no_of_numbers);
	if(no_of_numbers==0)
	{
	  printf("the number of numbers entered is zero");
	}
	int array[no_of_numbers];
	p=(int*)array;
	for(loop_var=0;loop_var<no_of_numbers;loop_var++)
	{
		printf("enter %d th number-	",loop_var+1);
		scanf("%d",&array[loop_var]);
	}
	pthread_create(&thread_for_average,NULL,averagefunction,p);//creating thread for calculating average of all the entered numbers 
	pthread_join(thread_for_average,NULL);
	pthread_create(&thread_for_maximum,NULL,maximumfunction,p);//creating thread for calculating maximum all the entered numbers 
	pthread_join(thread_for_maximum,NULL);
	pthread_create(&thread_for_minimum,NULL,minimumfunction,p);//creating thread for calculating minimum all the entered numbers
	pthread_join(thread_for_minimum,NULL);
	//p is passed as argument to the function whose thread is created
	printf("\nthe mean or average of the entered numbers is %d \n",average);
	printf("the minimum number among all the entered numbers is %d\n",minimum);
	printf("the maximum number among all the entered numbers is %d \n",maximum);
}
void *averagefunction(void *p) //this function is created as thread which calculates and prints average of entered numbers
{
	int loop_var1;
	for(loop_var1=0;loop_var1<no_of_numbers;loop_var1++)
	{
		average=average+(*(int *)p);
		p=p+4;
	}
	average=(average/no_of_numbers);
	return NULL;
}
void *maximumfunction(void *p)	//this function is created as thread which calculates and prints maximum of all entered numbers
{
	int loop_var2;
	for(loop_var2=0;loop_var2<no_of_numbers;loop_var2++)
	{
		if(*(int *)p>maximum)
		maximum=*(int *)p;
		p=p+4;
	}
	return NULL;
}
void *minimumfunction(void *p) //this function is created as thread which calculates and prints minimum of entered numbers
{
	int loop_var3;
	minimum=*(int *)p;
	for(loop_var3=0;loop_var3<no_of_numbers;loop_var3++)
	{
		if(*(int *)p<minimum)
		minimum=*(int *)p;
		p=p+4;
	}
	return NULL;
}
//question 25 code where interprocess comuunication is used
//two pipes are created in the program to transfer data between to processes
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main()
{
	int n;
	int mypipe1[2];
	int mypipe2[2];
	pipe(mypipe1);
	pipe(mypipe2);
	printf("enter the length of the string");
	scanf("%d",&n);
	char buf1[n];
	printf("enter string ");
	scanf("%s",buf1);
	void fun2()
	{
		int i,t;
		char buf2[n];
		read(mypipe1[0],buf2,n);
		for(i=0;i<n;i++)
		{
			t=buf2[i];
			if((t>=65)&&(t<=90))
			{
				t=t+32;
				buf2[i]=t;
			}
			else if((t>=97)&&(t<=122))
			{
				t=t-32;
				buf2[i]=t;
			}
		}
		write(mypipe2[1],buf2,n);
	}
	void fun1()
	{
		char buf3[n];
		write(mypipe1[1],buf1,n);
		fun2();
		read(mypipe2[0],buf3,n);
		printf("the final string is %s",buf3);
	}
	fun1();
}
