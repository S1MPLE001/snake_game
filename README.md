
#include<bits/stdc++.h>
#include<conio.h>
#include<time.h>
#include<stdlib.h>
#include<ctype.h>
#include<windows.h>
#include<process.h>
#define U 119
#define D 115
#define L 97
#define R 100

int length,len,bend_no;
char key;

struct domingo{
    int x,y,direction;
};
typedef struct domingo coordinate;
coordinate head, bend[500],food,body[30];
void GotoXY(int x, int y)
{  HANDLE a;
    COORD b;
    b.X = x;
    b.Y = y;
    a = GetStdHandle(STD_OUTPUT_HANDLE);
    SetConsoleCursorPosition(a,b);
 }
void Bend()
{
    int i,j,diff;
    for(i=bend_no;i>=0&&len<length;i--)
    {
            if(bend[i].x==bend[i-1].x)
            {
                diff=bend[i].y-bend[i-1].y;
                if(diff<0)
                    for(j=1;j<=(-diff);j++)
                    {
                        body[len].x=bend[i].x;
                        body[len].y=bend[i].y+j;
                        GotoXY(body[len].x,body[len].y);
                        printf("*");
                        len++;
                        if(len==length)
                            break;
                    }
                else if(diff>0)
                    for(j=1;j<=diff;j++)
                    {
                        body[len].x=bend[i].x;
                        body[len].y=bend[i].y-j;
                        GotoXY(body[len].x,body[len].y);
                        printf("*");
                        len++;
                        if(len==length)
                            break;
                    }
            }
        else if(bend[i].y==bend[i-1].y)
        {
            diff=bend[i].x-bend[i-1].x;
            if(diff<0)
                for(j=1;j<=(-diff)&&len<length;j++)
                {
                    body[len].x=bend[i].x+j;
                    body[len].y=bend[i].y;
                    GotoXY(body[len].x,body[len].y);
                        printf("*");
                   len++;
                   if(len==length)
                           break;
               }
           else if(diff>0)
               for(j=1;j<=diff&&len<length;j++)
               {
                   body[len].x=bend[i].x-j;
                   body[len].y=bend[i].y;
                   GotoXY(body[len].x,body[len].y);
                       printf("*");
                   len++;
                   if(len==length)
                       break;
               }
       }
   }
}
void Down()
{
    int i;
    for(i=0;i<=(head.y-bend[bend_no].y)&&len<length;i++)
    {
        GotoXY(head.x,head.y-i);
        {
            if(len==0)
                printf("v");
            else
                printf("*");
        }
        body[len].x=head.x;
        body[len].y=head.y-i;
        len++;
    }
    Bend();
    if(!kbhit())
        head.y++;
}
void Up()
{
   int i;
   for(i=0;i<=(bend[bend_no].y-head.y)&&len<length;i++)
   {
       GotoXY(head.x,head.y+i);
       {
           if(len==0)
               printf("^");
           else
               printf("*");
       }
       body[len].x=head.x;
       body[len].y=head.y+i;
       len++;
   }
   Bend();
   if(!kbhit())
       head.y--;
}
void eggs()
{
    if(head.x==food.x&&head.y==food.y)
    {
        length++;
        time_t a;
        a=time(0);
        srand(a);
        food.x=rand()%70;
        if(food.x<=10)
            food.x+=11;
        food.y=rand()%30;
        if(food.y<=10)

            food.y+=11;
    }
    else if(food.x==0)
    {
        food.x=rand()%70;
        if(food.x<=10)
            food.x+=11;
        food.y=rand()%30;
        if(food.y<=10)
            food.y+=11;
    }
}
void Left()
{
    int i;
    for(i=0;i<=(bend[bend_no].x-head.x)&&len<length;i++)
    {
        GotoXY((head.x+i),head.y);
       {
                if(len==0)
                    printf("<");
                else
                    printf("*");
        }
        body[len].x=head.x+i;
        body[len].y=head.y;
        len++;
    }
    Bend();
    if(!kbhit())
        head.x--;

}
void Right()
{
    int i;
    for(i=0;i<=(head.x-bend[bend_no].x)&&len<length;i++)
    {
        body[len].x=head.x-i;
        body[len].y=head.y;
        GotoXY(body[len].x,body[len].y);
        {
            if(len==0)
                printf(">");
            else
                printf("*");
        }
        len++;
    }
    Bend();
    if(!kbhit())
        head.x++;
}
void border()
{system("cls");
   int i;
   GotoXY(food.x,food.y);
       printf("P");
   for(i=10;i<71;i++)
   {GotoXY(i,10);
	printf("!");
	GotoXY(i,30);
printf("!");
   }
   for(i=10;i<31;i++)
   {GotoXY(10,i);
	printf("!");
	GotoXY(70,i);
	printf("!");
   }
}
void gotoxy(int x, int y)
{
COORD coord;
coord.X = x;
coord.Y = y;
SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord); // just to make console

}
void load()
{int r,q;
    gotoxy(36,14);
    printf("loading....\n");
    gotoxy(30,15);
    for(r=1; r<=20; r++)
    {for(q=0; q<=100000000; q++); //to display the character slowly
        printf("%c",177);
    }
}
void display()
{printf("welcome to chutiyaap press any key to play it");
    char ch;
    scanf("%c",&ch);
    if(ch=='@')
        exit(0);
}
void score(){
	int sco;
	GotoXY(20,8);
	sco=length-5;
	printf("SCORE : %d",sco);
}
void Delay(long double k)
{
    score();
    long double i;
    for(i=0;i<=(10000000);i++);
}
void ExitGame()
{
    int i,check=0;
    for(i=4;i<length;i++)
    {
        if(body[0].x==body[i].x&&body[0].y==body[i].y)
        {
            check++;
        }
        if(i==length||check!=0)
            break;
    }
    if(head.x<=10||head.x>=70||head.y<=10||head.y>=30||check!=0)
    {
            printf("\n\n\n\n\n\n\n\n\n\nAll lives completed\nBetter Luck Next Time!!!\nPress any key to quit the game\n");
            exit(0);
        }
    }
void Move()
{int i=0,a;
    do{
			eggs();
        fflush(stdin);
        len=0;
	for(i=0;i<30;i++)
	{body[i].x=0;
	body[i].y=0;
	if(i==length)
	break;
	}
        Delay(length);
        border();
	if(head.direction==R)
		Right();
		else if(head.direction==L)
		Left();
		else if(head.direction==D)
		Down();
		else if(head.direction==U)
		Up();
		ExitGame();
		}while(!kbhit());
    key=getch();
     a=getch();

    if(a==27){
	system("cls");
	exit(0);
	}

    if((key==R&&head.direction!=L&&head.direction!=R)||(key==L&&head.direction!=R&&head.direction!=L)||(key==U&&head.direction!=D&&head.direction!=U)||(key==D&&head.direction!=U&&head.direction!=D))
{
		bend_no++;
		bend[bend_no]=head;
		head.direction=key;
		if(key==U)
		head.y--;
		if(key==D)
			head.y++;
		if(key==R)
		head.x++;
		if(key==L)
		head.x--;
			Move();
			}

    else if(key==27)
		{

        system("cls");

        exit(0);

    }

    else{
printf("\a");
Move();
}
}
int main()
{
    char key;
    display();
    system("cls");
    load();
    length=5;
    head.x=25;
    head.y=20;
    head.direction=R;
    border();
    eggs();
    bend[0]=head;
    Move();

}

<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<CodeBlocks_project_file>
	<FileVersion major="1" minor="6" />
	<Project>
		<Option title="snake game" />
		<Option pch_mode="2" />
		<Option compiler="gcc" />
		<Build>
			<Target title="Debug">
				<Option output="bin/Debug/snake game" prefix_auto="1" extension_auto="1" />
				<Option object_output="obj/Debug/" />
				<Option type="1" />
				<Option compiler="gcc" />
				<Compiler>
					<Add option="-g" />
				</Compiler>
			</Target>
			<Target title="Release">
				<Option output="bin/Release/snake game" prefix_auto="1" extension_auto="1" />
				<Option object_output="obj/Release/" />
				<Option type="1" />
				<Option compiler="gcc" />
				<Compiler>
					<Add option="-O2" />
				</Compiler>
				<Linker>
					<Add option="-s" />
				</Linker>
			</Target>
		</Build>
		<Compiler>
			<Add option="-Wall" />
			<Add option="-fexceptions" />
		</Compiler>
		<Unit filename="main.cpp" />
		<Extensions>
			<code_completion />
			<envvars />
			<debugger />
		</Extensions>
	</Project>
</CodeBlocks_project_file>

# depslib dependency file v1.0
1511199381 source:e:\snake game\main.cpp
	<bits/stdc++.h>
	<conio.h>
	<time.h>
	<stdlib.h>
	<ctype.h>
	<windows.h>
	<process.h>


<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<CodeBlocks_layout_file>
	<FileVersion major="1" minor="0" />
	<ActiveTarget name="Debug" />
	<File name="main.cpp" open="1" top="1" tabpos="2" split="0" active="1" splitpos="0" zoom_1="0" zoom_2="0">
		<Cursor>
			<Cursor1 position="227" topLine="0" />
		</Cursor>
	</File>
</CodeBlocks_layout_file>
