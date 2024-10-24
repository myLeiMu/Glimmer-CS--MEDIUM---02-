# Glimmer-CS--MEDIUM---02-
#CS-MEDIUM-02
##课前
1.课前准备中的说明我没太读懂，不知道是将读文件后修改，还是直接修改文件本身，也就是写文件，我这里是采用修改文件本身的办法。
~~~C
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
//转化为十进制
char*ten(char a[])
{   int b;
    char intpart[32],fracpart[32];
    sscanf(a,"%[^.].%s",intpart,fracpart);
    b=atoi(intpart);//将字符变成数
    int num[32],i=0,j=1,k=0,p=0,l=0;
    int f=2,y=0;
    while(b>0)
    {   
        num[i++]=b%10;
        b/=10;
    }
    while(i-1>=p)
    {
        k+=j*num[p++];
        j*=2;
    }
    int len=strlen(fracpart);
    int lens=1,tens=1;
    while (lens<=len)
    {
        tens*=10;
        lens++;
    }
    char intpart1[32],fracpart1[32];
    while(fracpart[l]!='\0')
    {
        if(fracpart[l++]=='1')
        y+=tens/f;
        f*=2;
    }
    char*result=(char*)malloc(500);
    sprintf(intpart1,"%d",k);
    sprintf(fracpart1,"%d",y);
    char* pot=".";
    result[0]='\0';
    strcat(intpart1,pot);
    strcat(intpart1,fracpart1);
    char*D="D";
    strcat(intpart1,D);
    strcat(result,intpart1);
    return  result;
}
int main()
{  
    FILE *file=fopen("D:\\A\\CS_M_02.txt","r+");
    if(NULL==file)
    {
        printf("Error");
    }
    char temp[500]="\0";
    char buffer[500];
    char*a=buffer;
    char*d=buffer;
    char *y,*y2;
    while(fgets(buffer,200,file)!= NULL)
    {   if((a=strstr(buffer,"B"))!=NULL)
        {   char h[50];
            int p=0;
            d=strstr(buffer," ");
            y2=y=++d;
        while(y<a)
        {
        h[p++]=*y;
            y++;
        }
        h[p]='\0';
        char*x=ten(h);
        *y2='\0';
        strcat(buffer,x);
        strcat(buffer,"\n");
        free(x);
        }strcat(temp,buffer);
    }
    freopen("D:\\A\\CS_M_02.txt", "w+", file);  
    fwrite(temp, sizeof(char), strlen(temp), file);
    fclose(file);
    printf("yes!");
}
~~~
但我现在知道竟然有个strtol函数，果然C语言还是方便的，不过这里应该也不希望使用这个函数吧。
这里我不清楚是截什么的图就放的改了后的文件
![image](https://github.com/user-attachments/assets/895244a2-346d-46ec-8a51-453413ad08a1)


##任务1
1.因为浮点数在计算机中是用二进制表示的，但是一些十进制小数在二进制中无法精确表示，是无线循环的，也可以说存储并不连续，小数会被截断，从而导致精度损失。
2.关于如何获得精确值，我首先的想法就是像大数运算那样，运用数组，字符数组来存储数据。在我的了解后，Java中的BigInteger和BigDecimal类似乎就是按照的这种方式来精确表示定点数，而python中的Decimal块则好像是直接转而用十进制表示，从而避开了二进制表示小数会截断的情况，至于C语言gmp.h头文件，它采用的也是类数组的方式，用块的存储机制来分解数据，再用指数来控制小数点位置，而且它是动态数组的方式，更加灵活。
3.
~~~c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int two(int a,float f,char c[])
{   int i=0,j=0,l=0;
    int k=0;
    float h=0.5;
    int b[32];
    while(a>0)
    {   
        b[i++]=a%2;
        a/=2;
    }
    for(int p=--i;j<=i;i--)
    {   
        if(i==p)
        {
            k+=b[i];
        }else
        k=k*10+b[i];
    }
    while(f>0)
    {   f-=h;
        if(f>=0)
        c[l]='1';
        else
        {
        c[l]='0';
        f+=h;
        }
        l++;
        h/=2;
    }
    c[l]='\0';
    return k;
}
float ten(int b,char c[])
{   
    int a[32],i=0,j=1,k=0,p=0,l=0;
    float f=0.5,y=0.0;
    while(b>0)
    {   
        a[i++]=b%10;
        b/=10;
    }
    while(i-1>=p)
    {
        k+=j*a[p++];
        j*=2;
    }
    while(c[l]!='\0')
    {
        if(c[l++]=='1')
        y+=f;
        f/=2;
    }
    return k+y;
}
/*在将一个十进制的数转化为二进制的过程中，若存在小数部分，转化为二进制无法保证精度，
所以我采用将整数和小数分割的方式，这种方式能存储转化后的数,之后既可以输出也能用于计
算等，因为这里不清楚转化的目的。但对于二进制转十进制，由于精度的原因；我试了几个方
法都不能成功转化，我感觉只有一开始就以字符的形式保存数据才能保证之后的转换中不会出
现由于精度而导致的问题。这里我之前写过整数的办法，所以整数就没有变成字符串。
*/
int main()
{   float a,c;
    int b;
    char f[64],intpart[32],frac_part1[32];
    int int_part;
    char frac_part[32];
    printf("turn ten into two:");
    scanf("%f",&a);
    int_part=a;
    a-=int_part;
    int_part=two(int_part,a,frac_part);
    printf("%d",int_part);
    if(a!=0)
    printf(".");
    printf("%s",frac_part);
    printf("\nturn two into ten:");
    scanf("%s",&f);
    if(strchr(f,'.')!=NULL){
    sscanf(f,"%[^.].%s",intpart,frac_part1);
    int_part=atoi(intpart);
    c=ten(int_part,frac_part1);
    printf("%f",c);
    }
    else {
        b=ten(int_part,NULL);
        printf("%d",b);}
}
~~~
##任务2
###step1
~~~c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
typedef struct{
    char type;
    char int_part[33];
    char frac_part[32];
}PointFixedNum;
int foundend(char a[])
{
    int b=0;
    while (a[b]!='\0')
    {
        b++;
    }
    return b;
}
void init(char a[],PointFixedNum* point)
{   
    if(a[0]=='-')
    {
        a++;
        point->type='-';

    }else point->type='\0';
    int end=foundend(a);
    if(a[end-1]=='\n')
    end--;
    if(a[end-1]=='B')
    {
    a[end-1]='\0';
    int b;
    char intpart[32],fracpart[32];
    sscanf(a,"%[^.].%s",intpart,fracpart);
    b=atoi(intpart);
    int num[32],i=0,j=1,k=0,p=0,l=0;
    int f=2,y=0;
    while(b>0)
    {   
        num[i++]=b%10;
        b/=10;
    }
    while(i-1>=p)
    {
        k+=j*num[p++];
        j*=2;
    }
    int len=strlen(fracpart);
    int lens=1,tens=1;
    while (lens<=len)
    {
        tens*=10;
        lens++;
    }
    while(fracpart[l]!='\0')
    {
        if(fracpart[l++]=='1')
        y+=tens/f;
        f*=2;
    }
    sprintf(point->int_part,"%d",k);
    sprintf(point->frac_part,"%d",y);
    }else
    {a[end-1]='\0';
    sscanf(a,"%[^.].%s",point->int_part,point->frac_part);
    }
}
int main()
{
    char num[65];
    scanf("%s",num);
    PointFixedNum first;
    init(num,&first);
    printf("%c%s.%sD",first.type,first.int_part,first.frac_part);
}
~~~
![image](https://github.com/user-attachments/assets/90359c5d-23a4-4d65-8bb3-87fb76a80623)

###step2
~~~c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int N=0;
typedef struct{
    char type;
    char int_part[33];
    char frac_part[32];
}PointFixedNum;
int foundend(char a[])
{
    int b=0;
    while (a[b]!='\0')
    {
        b++;
    }
    return b;
}
void minus(PointFixedNum* first1,PointFixedNum *second1 ,char result[])
{
    char*a=first1->int_part,*b=second1->int_part;
    char*f1=first1->frac_part,*f2=second1->frac_part;
    int pointa=strlen(f1);
    int pointb=strlen(f2);
    int pot=pointa>pointb?pointa:pointb;
    if(pointa>pointb)
    {   while(pointa>pointb)
        f2[pointb++]='0';
        f2[pointb]='\0';
    }else
    {
        while (pointa<pointb)
        f1[pointa++]='0';
        f1[pointa]='\0';
    }
    strcat(a,f1);
    strcat(b,f2);
    int len1=strlen(a);
    int len2=strlen(b);
    int borrow=0;
    int c=0;
    int i=len1-1;
    int j=len2-1;
    if (len1<len2||(len1 == len2 && strcmp(a, b)<0)) {
        N++;
        char*x=a;
        a=b;
        b=x;
        int t=len1;
        len1=len2;
        len2=t;
    }
    while (i>=0||j>=0)
    {   
        int diff=borrow;
        diff+=a[i--]-'0';
        if(j>=0)
        diff-=b[j--]-'0';
        if(diff<0)
        {
            diff+=10;
            borrow=-1;
        }else borrow=0;
        result[c++]=diff+'0';
    }
    while (c>1&&result[c-1]=='0')
    {
        c--;
    }
    result[c]='\0';
    int cc=c;
    c++;
    while(pot-1!=cc)
    {   
        result[cc+1]=result[cc];
        cc--;
    }
    result[pot]='.';
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
}
void plus(PointFixedNum  *first1,PointFixedNum *second1 ,char result[])
{   char* a=first1->int_part,*b=second1->int_part;
    char *f1=first1->frac_part,*f2=second1->frac_part;
    int pointa=strlen(f1);
    int pointb=strlen(f2);
    int pot=pointa>pointb?pointa:pointb;
    if(pointa>pointb)
    {   while(pointa>pointb)
        f2[pointb++]='0';
        f2[pointb]='\0';
    }else
    {
        while (pointa<pointb)
        f1[pointa++]='0';
        f1[pointa]='\0';
    }
    strcat(a,f1);
    strcat(b,f2);
    int len1=strlen(a);
    int len2=strlen(b);
    char result1[50];
    int add=0;
    int c=0;
    int i=len1-1;
    int j=len2-1;
    while (i>=0||j>=0||add)
    {   int sum=0;
        if(i>=0)
        {
            sum+=a[i--]-'0';
        }
        if(j>=0)
        {
            sum+=b[j--]-'0';
        }
        sum+=add;
        result1[c++]=(sum%10)+'0';
        add=sum/10;
    }
    result1[c++]='\0';
    int h=0,p=0;
    while(c!=p-1)
    {   if(h==pot)
        result[h++]='.';
        result[h++]=result1[p++];
    }
    for(int d=0;d<c/2;d++)
    {   
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
}
void multiply(PointFixedNum* first1,PointFixedNum *second1 ,char result[])
{
    char*a=first1->int_part,*b=second1->int_part;
    char*f1=first1->frac_part,*f2=second1->frac_part;
    int pointa=strlen(f1);
    int pointb=strlen(f2);
    int pot=pointa+pointb;
    strcat(a,f1);
    strcat(b,f2);
    int len1=strlen(a);
    int len2=strlen(b);
    int res[100]={0};
    int k=0;
    for(int i=len1-1;i>=0;i--)
    {
        for(int j=len2-1;j>=0;j--)
        {   k=len1+len2-i-j-2;
            res[k]+=(a[i]-'0')*(b[j]-'0');
            res[k+1]+=res[k]/10;
            res[k]%=10;
        }
    }
    for(int i=0;i<=k+1;i++)
    {
        result[i]=res[i]+'0';
    }
    if(result[k+1]=='0')
    result[k+1]='\0';
    else result[++k+1]='\0';
    int cc=1+k++;
    while(pot-1!=cc)
    {   
        result[cc+1]=result[cc];
        cc--;
    }
    result[pot]='.';
    for(int d=0;d<(k+1)/2;d++)
    {
        char num=result[d];
        result[d]=result[k-d];
        result[k-d]=num;
    }
}
void init(char a[],PointFixedNum* point)
{   
    if(a[0]=='-')
    {
        a++;
        point->type='-';
    }else point->type='\0';
    int end=foundend(a);
    if(a[end-1]=='\n')
    end--;
    if(a[end-1]=='B')
    {
    a[end-1]='\0';
    int b;
    char intpart[32],fracpart[32];
    sscanf(a,"%[^.].%s",intpart,fracpart);
    b=atoi(intpart);
    int num[32],i=0,j=1,k=0,p=0,l=0;
    int f=2,y=0;
    while(b>0)
    {   
        num[i++]=b%10;
        b/=10;
    }
    while(i-1>=p)
    {
        k+=j*num[p++];
        j*=2;
    }
    int len=strlen(fracpart);
    int lens=1,tens=1;
    while (lens<=len)
    {
        tens*=10;
        lens++;
    }
    while(fracpart[l]!='\0')
    {
        if(fracpart[l++]=='1')
        y+=tens/f;
        f*=2;
    }
    sprintf(point->int_part,"%d",k);
    sprintf(point->frac_part,"%d",y);
    }else
    {a[end-1]='\0';
    sscanf(a,"%[^.].%s",point->int_part,point->frac_part);
    }
}
int main()
{   char result[65];
    char a[100];
    char n1[50],n2[50];
    fgets(a,sizeof(a),stdin);
    char* search=strpbrk(a,"+-*");
    strncpy(n1,a,search-a-1);
    n1[search-a-1]='\0';
    char order=*search;
    strcpy(n2,search+2);
    PointFixedNum first,second;
    init(n1,&first);
    init(n2,&second);
    char resulttype='\0';
    switch (order)
    {
    case '+':
        if(first.type!='-'&&second.type=='-')
        {
            minus(&first,&second,result);
        }else if (first.type=='-'&&second.type!='-')
        {
            minus(&first,&second,result);
        }
        
        else{ 
            if(first.type=='-'&&second.type=='-')
            N++;
            plus(&first,&second,result);
        }
        break;
    case '-':
        if(first.type!='-'&&second.type=='-')
        {
            plus(&first,&second,result);
        }else if(first.type=='-'&&second.type!='-')
        {   
            N++;
            plus(&first,&second,result);
        }else
        minus(&first,&second,result);
        break;
    case '*':
        if(first.type!='-'&&second.type=='-')
        N++;
        if(first.type=='-'&&second.type!='-')
        N++;
        multiply(&first,&second,result);
        break;
    }
    if(N==1)
    resulttype='-';
    printf("%c%sD",resulttype,result);
}
~~~
![image](https://github.com/user-attachments/assets/38d4039d-c836-42ca-b47d-4a44e6df3d2c)

##任务三
~~~c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int N=0;
typedef struct{
    char type;
    char int_part[33];
    char frac_part[32];
}PointFixedNum;
int foundend(char a[])
{
    int b=0;
    while (a[b]!='\0')
    {
        b++;
    }
    return --b;
}
void minus(PointFixedNum* first1,PointFixedNum *second1 ,char result[])
{
    char*a=first1->int_part,*b=second1->int_part;
    char*f1=first1->frac_part,*f2=second1->frac_part;
    int pointa=strlen(f1);
    int pointb=strlen(f2);
    int pot=pointa>pointb?pointa:pointb;
    if(pointa>pointb)
    {   while(pointa>pointb)
        f2[pointb++]='0';
        f2[pointb]='\0';
    }else
    {
        while (pointa<pointb)
        f1[pointa++]='0';
        f1[pointa]='\0';
    }
    strcat(a,f1);
    strcat(b,f2);
    int len1=strlen(a);
    int len2=strlen(b);
    int borrow=0;
    int c=0;
    int i=len1-1;
    int j=len2-1;
    if (len1<len2||(len1 == len2 && strcmp(a, b)<0)) {
        N++;
        char*x=a;
        a=b;
        b=x;
        int t=len1;
        len1=len2;
        len2=t;
    }
    while (i>=0||j>=0)
    {   
        int diff=borrow;
        diff+=a[i--]-'0';
        if(j>=0)
        diff-=b[j--]-'0';
        if(diff<0)
        {
            diff+=10;
            borrow=-1;
        }else borrow=0;
        result[c++]=diff+'0';
    }
    while (c>1&&result[c-1]=='0')
    {
        c--;
    }
    result[c]='\0';
    int cc=c;
    c++;
    while(pot-1!=cc)
    {   
        result[cc+1]=result[cc];
        cc--;
    }
    result[pot]='.';
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
}
void plus(PointFixedNum  *first1,PointFixedNum *second1 ,char result[])
{   char* a=first1->int_part,*b=second1->int_part;
    char *f1=first1->frac_part,*f2=second1->frac_part;
    int pointa=strlen(f1);
    int pointb=strlen(f2);
    int pot=pointa>pointb?pointa:pointb;
    if(pointa>pointb)
    {   while(pointa>pointb)
        f2[pointb++]='0';
        f2[pointb]='\0';
    }else
    {
        while (pointa<pointb)
        f1[pointa++]='0';
        f1[pointa]='\0';
    }
    strcat(a,f1);
    strcat(b,f2);
    int len1=strlen(a);
    int len2=strlen(b);
    char result1[50];
    int add=0;
    int c=0;
    int i=len1-1;
    int j=len2-1;
    while (i>=0||j>=0||add)
    {   int sum=0;
        if(i>=0)
        {
            sum+=a[i--]-'0';
        }
        if(j>=0)
        {
            sum+=b[j--]-'0';
        }
        sum+=add;
        result1[c++]=(sum%10)+'0';
        add=sum/10;
    }
    result1[c++]='\0';
    int h=0,p=0;
    while(c!=p-1)
    {   if(h==pot)
        result[h++]='.';
        result[h++]=result1[p++];
    }
    for(int d=0;d<c/2;d++)
    {   
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
}
void multiply(PointFixedNum* first1,PointFixedNum *second1 ,char result[])
{
    char*a=first1->int_part,*b=second1->int_part;
    char*f1=first1->frac_part,*f2=second1->frac_part;
    int pointa=strlen(f1);
    int pointb=strlen(f2);
    int pot=pointa+pointb;
    strcat(a,f1);
    strcat(b,f2);
    int len1=strlen(a);
    int len2=strlen(b);
    int res[100]={0};
    int k=0;
    for(int i=len1-1;i>=0;i--)
    {
        for(int j=len2-1;j>=0;j--)
        {   k=len1+len2-i-j-2;
            res[k]+=(a[i]-'0')*(b[j]-'0');
            res[k+1]+=res[k]/10;
            res[k]%=10;
        }
    }
    for(int i=0;i<=k+1;i++)
    {
        result[i]=res[i]+'0';
    }
    if(result[k+1]=='0')
    result[k+1]='\0';
    else result[++k+1]='\0';
    int cc=1+k++;
    while(pot-1!=cc)
    {   
        result[cc+1]=result[cc];
        cc--;
    }
    result[pot]='.';
    for(int d=0;d<(k+1)/2;d++)
    {
        char num=result[d];
        result[d]=result[k-d];
        result[k-d]=num;
    }
}
void init(char a[],PointFixedNum* point)
{   
    if(a[0]=='-')
    {
        a++;
        point->type='-';
    }else point->type='\0';
    int end=foundend(a);
    if(a[end-1]=='\n')
    end--;
    if(a[end-1]=='B')
    {
    a[end-1]='\0';
    int b;
    char intpart[32],fracpart[32];
    sscanf(a,"%[^.].%s",intpart,fracpart);
    b=atoi(intpart);
    int num[32],i=0,j=1,k=0,p=0,l=0;
    int f=2,y=0;
    while(b>0)
    {   
        num[i++]=b%10;
        b/=10;
    }
    while(i-1>=p)
    {
        k+=j*num[p++];
        j*=2;
    }
    int len=strlen(fracpart);
    int lens=1,tens=1;
    while (lens<=len)
    {
        tens*=10;
        lens++;
    }
    while(fracpart[l]!='\0')
    {
        if(fracpart[l++]=='1')
        y+=tens/f;
        f*=2;
    }
    sprintf(point->int_part,"%d",k);
    sprintf(point->frac_part,"%d",y);
    }else
    {a[end-1]='\0';
    sscanf(a,"%[^.].%s",point->int_part,point->frac_part);
    }
}
char* function(char n1[],char a[])
{   char result[65];
    char n2[50];
    char order=a[0];
    strcpy(n2,a+2);
    PointFixedNum first,second;
    init(n1,&first);
    init(n2,&second);
    char resulttype[100]="\0";
    switch (order)
    {
    case '+':
        if(first.type!='-'&&second.type=='-')
        {
            minus(&first,&second,result);
        }else if (first.type=='-'&&second.type!='-')
        {
            minus(&first,&second,result);
        }
        
        else{ 
            if(first.type=='-'&&second.type=='-')
            N++;
            plus(&first,&second,result);
        }
        break;
    case '-':
        if(first.type!='-'&&second.type=='-')
        {
            plus(&first,&second,result);
        }else if(first.type=='-'&&second.type!='-')
        {   
            N++;
            plus(&first,&second,result);
        }else
        minus(&first,&second,result);
        break;
    case '*':
        if(first.type!='-'&&second.type=='-')
        N++;
        if(first.type=='-'&&second.type!='-')
        N++;
        multiply(&first,&second,result);
        break;
    }
    if(N%2==1)
    strcat(resulttype,"-");
    strcat(resulttype,result);
    char* final=resulttype;
    return final;
}
int main()
{  
    FILE *file=fopen("D:\\A\\CS_M_02.txt","r+");
    if(NULL==file)
    {
        printf("Error");
    }
    char buffer[100];
    char a[64]="\0";
    while(fgets(buffer,sizeof(buffer),file)!= NULL)
    {  
        char* search=strpbrk(buffer,"+-*");
        if(search==NULL)
        {
        strcpy(a,buffer);
        continue;
        }
        char*b=function(a,buffer);
        strcpy(a,b);
    }
    fclose(file);
    printf("%s",a);
    printf("yes!");
}
~~~

![屏幕截图 2024-10-24 133026](https://github.com/user-attachments/assets/3f217c05-4685-4267-bf38-51f6c9922f3e)

我这里就是真的按物理次序进行的连续计算，就是没有管乘除的优先级，因为我感觉文档的格式就是希望一行一行地进行，
所以我就没有再设置一个判断符号再进行先后运算的过程。
但如果真就是按照运算次序来的话，我可能就会先直接读入所有内容，然后先找到“*”的位置，将前一个数与后一个数相乘，然后把这些数据储存起来，再找下一个，我会用到strstr函数，等找不到时再进行加减法运算，因为是只剩加减所以次序不重要，最后在加之前乘出的结果就行。
