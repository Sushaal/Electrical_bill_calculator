# Electrical_bill_calculator

#include<stdio.h>
#include<conio.h>
#include<stdlib.h>
#include<windows.h>
#include<string.h>
void gotoxy(int ,int );
void menu();
void view();
void add();
void search();
void deleterec();
struct customer
{
    char meterno[20];
    char name[20];
    int units;
    float total;
};
int main()
{
    gotoxy(15,8);
    printf("<--:Electricity bill:-->");
    gotoxy(19,15);
    printf("Press any key to continue.");
    getch();
    menu();
    return 0;
}
void menu()
{
    int choice;
    system("cls");
    gotoxy(10,3);
    printf("<--:MENU:-->");
    gotoxy(10,5);
    printf("Enter appropriate number to perform following task.");
    gotoxy(10,7);
    printf("1 : Add Record.");
    gotoxy(10,8);
    printf("2 : view Record.");
    gotoxy(10,9);
    printf("3 : search Record.");
    gotoxy(10,10);
    printf("4 : Delete Record.");
    gotoxy(10,11);
    printf("5 : Exit.");
    gotoxy(10,12);
    printf("Enter your choice.");
    scanf("%d",&choice);
    switch(choice)
    {
    case 1:
        add();
        break;

    case 2:
        view();
        break;

    case 3:
        search();
        break;

    case 4:
        deleterec();
        break;
    case 5:
        exit(1);
        break;

    default:
        gotoxy(10,17);
        printf("Invalid Choice.");
    }
}
void add()
{
    FILE *fp;
    struct customer cus;
    char another ='y';
    system("cls");
    fp = fopen("vish.txt","ab+");
    if(fp == NULL){
        gotoxy(10,5);
        printf("Error opening file");
        exit(1);
    }
    fflush(stdin);
    while(another == 'y')
    {
        gotoxy(10,3);
        printf("<--:ADD RECORD:-->");
        gotoxy(10,5);
        printf("Enter details of customer");
        gotoxy(10,7);
        printf("Enter meter number : ");
        gets(cus.meterno);
        gotoxy(10,8);
        printf("Enter Name: ");
        gets(cus.name);
        gotoxy(10,9);
        printf("Enter no of units: ");
        scanf("%d",&cus.units);
        int k=cus.units;
        fflush(stdin);
        int s=35;
        if(k>0&&k<50)
        {
            cus.total=(k*1.35)+s;
        }
        else if(k>=50&&k<150)
        {
            cus.total=(k*2.15)+s;
        }
        else if(k>=150&&k<300)
        {
           cus.total=(k*3)+s;
        }
        else if(k>=300&&k<500)
        {
            cus.total=(k*3.5)+s;
        }
        else if(k>=500&&k<1000)
        {
            cus.total=(k*6)+s;
        }
        else{
            printf("invalid units");
        }
        gotoxy(10,10);
        printf("total bill:");
        printf("%f",cus.total);
        gotoxy(10,11);
        fflush(stdin);
        fwrite(&cus,sizeof(cus),1,fp);
        gotoxy(10,13);
        printf("Want to add of another record? Then press 'y' else 'n'.");
        fflush(stdin);
        another = getch();
        system("cls");
        fflush(stdin);
    }
    fclose(fp);
    gotoxy(10,14);
    printf("Press any key to continue.");
    getch();
    menu();
}
void view()
{
    FILE *fp;
    int i=1,j;
    struct customer cus;
    system("cls");
    gotoxy(10,3);
    printf("<--:VIEW RECORD:-->");
    gotoxy(10,5);
    printf("S.No   meter number       name        number of units         Bill");
    gotoxy(10,6);
    printf("--------------------------------------------------------------------");
    fp = fopen("vish.txt","rb+");
    if(fp == NULL){
        gotoxy(10,8);
        printf("Error opening file.");
        exit(1);
    }
    j=8;
    while(fread(&cus,sizeof(cus),1,fp) == 1){
        gotoxy(10,j);
        printf("%-7d%-18s%-15s%-22d%-22f%",i,cus.meterno,cus.name,cus.units,cus.total);
        //printf("%d%s%s%d%s%s",i,std.name,std.mobile,std.rollno,std.course,std.branch);
        i++;
        j++;
    }
    fclose(fp);
    gotoxy(10,j+3);
    printf("Press any key to continue.");
    getch();
    menu();
}

void search()
{
    int p=0;
    FILE *fp;
    struct customer cus;
    char cusname[20];
    system("cls");
    gotoxy(11,3);
    printf("<--:SEARCH RECORD:-->");
    gotoxy(11,5);
    printf("Enter name of customer : ");
    fflush(stdin);
    gets(cusname);
    fp = fopen("vish.txt","rb+");
    if(fp == NULL){
        gotoxy(11,6);
        printf("Error opening file");
        exit(1);
    }
    while(fread(&cus,sizeof(cus),1,fp ) == 1){
        if(strcmp(cusname,cus.name)== 0){
            gotoxy(11,8);
            printf("meter number: %s",cus.meterno);
            gotoxy(11,9);
            printf("name : %s",cus.name);
            gotoxy(11,10);
            printf("number of units : %d",cus.units);
            gotoxy(11,11);
            printf("Bill: %f",cus.total);
            p=1;
        }
    }
    fclose(fp);
    gotoxy(11,17);
    if(p==0)
    {
        printf("No data found with name:%s\n",cusname);
    }
    gotoxy(11,18);
    printf("Press any key to continue.");
    getch();
    menu();
}
void deleterec()
{
    char cusname[20];
    FILE *fp,*ft;
    struct customer cus;
    system("cls");
    gotoxy(10,3);
    printf("<--:DELETE RECORD:-->");
    gotoxy(10,5);
    printf("Enter name of customer to delete record : ");
    fflush(stdin);
    gets(cusname);
    fp = fopen("vish.txt","rb+");
    if(fp == NULL){
        gotoxy(10,6);
        printf("Error opening file");
        exit(1);
    }
    ft = fopen("temp.txt","wb+");
    if(ft == NULL){
        gotoxy(10,6);
        printf("Error opening file");
        exit(1);
    }
    while(fread(&cus,sizeof(cus),1,fp) == 1){
        if(strcmp(cusname,cus.name)!=0)
            fwrite(&cus,sizeof(cus),1,ft);
    }
    fclose(fp);
    fclose(ft);
    remove("vish.txt");
    rename("temp.txt","vish.txt");
    gotoxy(10,10);
    printf("Press any key to continue.");
    getch();
    menu();
}

void gotoxy(int x,int y)
{
        COORD c;
        c.X=x;
        c.Y=y;
        SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),c);

}
