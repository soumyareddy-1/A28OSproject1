#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>
void *averagefunction(void *);
void *maximumfunction(void *);
void *minimumfunction(void *);
int no_of_numbers;
void *p;
int main()
{
	int loop_var;
	pthread_t thread_for_average,thread_for_maximum,thread_for_minimum;
	printf("enter no of numbers");
	scanf("%d",&no_of_numbers);
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
	
}
void *averagefunction(void *p) //this function is created as thread which calculates and prints average of entered numbers
{
	int loop_var1;
	int average=0;
	for(loop_var1=0;loop_var1<no_of_numbers;loop_var1++)
	{
		average=average+(*(int *)p);
		p=p+4;
	}
	average=(average/no_of_numbers);
	printf("\nthe mean or average of the entered numbers is %d \n",average);
	return NULL;
}
void *maximumfunction(void *p)	//this function is created as thread which calculates and prints maximum of all entered numbers
{
	int loop_var2;
	int maximum=0;
	for(loop_var2=0;loop_var2<no_of_numbers;loop_var2++)
	{
		if(*(int *)p>maximum)
		maximum=*(int *)p;
		p=p+4;
	}
	printf("the maximum number among all the entered numbers is %d \n",maximum);
	return NULL;
}
void *minimumfunction(void *p) //this function is created as thread which calculates and prints minimum of entered numbers
{
	int loop_var3;
	int minimum;
	minimum=*(int *)p;
	for(loop_var3=0;loop_var3<no_of_numbers;loop_var3++)
	{
		if(*(int *)p<minimum)
		minimum=*(int *)p;
		p=p+4;
	}
	printf("the minimum number among all the entered numbers is %d\n",minimum);
	return NULL;
}
