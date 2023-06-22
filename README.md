#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<conio.h>
#include <windows.h>


struct patient
{
    int id;
    char patientName[50];
    char patientAddress[50];
    char disease[50];
    char date[12];
} p;

struct ICU
{

    char name[50];
    char address[50];
    int ICU;
    char date[12];
} icu;

FILE *fp;

int main()
{

    int ch;

    while(1)
    {
        system("cls");
        setColor(4);
        printf("\n\n\t\t\t\t\t\t\t<== Hospital Management System ==>\n");
        printf("\n\t\t\t\t\t\t\t----------------------------------\n");
        printf("\n\n\t\t\t\t\t\t\t1.Admit Patient\n");
        printf("\t\t\t\t\t\t\t2.Patient List\n");
        printf("\t\t\t\t\t\t\t3.Discharge Patient\n");
        printf("\t\t\t\t\t\t\t4.Add ICU\n");
        printf("\t\t\t\t\t\t\t5.ICU List\n");
        printf("\t\t\t\t\t\t\t6.Delete ICU\n");
        printf("\t\t\t\t\t\t\t7.About Us\n");
        printf("\t\t\t\t\t\t\t0.Exit\n\n");
        setColor(238);
        printf("\n\n\t\t\t\t\t\t\tEnter your choice: ");
        scanf("%d", &ch);
        system("cls");

        switch(ch)
        {
        case 0:
            setColor();
            setColor(52);
            printf("\n\n\t\t\t\t\t\t\tTHANK YOU!\n");
            exit(0);

        case 1:
            admitPatient();
            break;

        case 2:
            patientList();
            break;

        case 3:
            dischargePatient();
            break;

        case 4:
            addICU();
            break;

        case 5:
            ICUList();
            break;

        case 6:
            deleteICU();
            break;

        case 7:
            aboutus();
            break;


        default:
            setColor(4);
            printf("\n\t\t\t\t\t\t\tInvalid Choice...\n\n");

        }
        setColor(238);
        printf("\n\n\n\n\n\t\t\t\t\t\t\tPress Any Key To Continue...");
        getch();
    }

    return 0;
}

void admitPatient()
{
    system("cls");
    printf("\n\n\t\t\t\t\t\t\t<== Admit Patient==>\n\n");
    char myDate[12];
    time_t t = time(NULL);
    struct tm tm = *localtime(&t);
    sprintf(myDate, "%02d/%02d/%d", tm.tm_mday, tm.tm_mon+1, tm.tm_year + 1900);
    strcpy(p.date, myDate);

    fp = fopen("patient.txt", "ab");

    printf("\t\t\t\t\t\t\tEnter Patient id: ");
    scanf("%d", &p.id);

    printf("\t\t\t\t\t\t\tEnter Patient name: ");
    fflush(stdin);
    gets(p.patientName);

    printf("\t\t\t\t\t\t\tEnter Patient Address: ");
    fflush(stdin);
    gets(p.patientAddress);

    printf("\t\t\t\t\t\t\tEnter Patient Disease: ");
    fflush(stdin);
    gets(p.disease);
    setColor(2);
    printf("\n\t\t\t\t\t\t\tPatient Added Successfully");

    fwrite(&p, sizeof(p), 1, fp);
    fclose(fp);
}

void patientList()
{
    setColor(235);
    system("cls");
    printf("\n\n\t\t\t\t\t\t\t<== Patient List ==>\n\n");
    printf("%-10s %-30s %-30s %-20s %s\n", "Id", "Patient Name", "Address", "Disease", "Date");
    printf("----------------------------------------------------------------------------------------------------------\n");

    fp = fopen("patient.txt", "rb");
    while(fread(&p, sizeof(p), 1, fp) == 1)
    {
        printf("%-10d %-30s %-30s %-20s %s\n", p.id, p.patientName, p.patientAddress, p.disease, p.date);
    }

    fclose(fp);
}


void dischargePatient()

{
    int id, f=0;
    system("cls");
    printf("\n\n\t\t\t\t\t\t\t<== Discharge Patient ==>\n\n");
    printf("\t\t\t\t\t\t\tEnter Patient id to discharge: ");
    scanf("%d", &id);

    FILE *ft;

    fp = fopen("patient.txt", "rb");
    ft = fopen("temp.txt", "wb");

    while(fread(&p, sizeof(p), 1, fp) == 1)
    {

        if(id == p.id)
        {
            f=1;
        }
        else
        {
            fwrite(&p, sizeof(p), 1, ft);
        }
    }

    if(f==1)
    {
        setColor(2);
        printf("\n\n\t\t\t\t\t\t\tPatient Discharged Successfully.");
    }
    else
    {
        setColor(4);
        printf("\n\n\t\t\t\t\t\t\tRecord Not Found !");
    }

    fclose(fp);
    fclose(ft);

    remove("patient.txt");
    rename("temp.txt", "patient.txt");

}

void addICU()
{

    char myDate[12];
    time_t t = time(NULL);
    struct tm tm = *localtime(&t);
    sprintf(myDate, "%02d/%02d/%d", tm.tm_mday, tm.tm_mon+1, tm.tm_year + 1900);
    strcpy(icu.date, myDate);

    int f=0;

    system("cls");
    printf("\n\n\t\t\t\t\t\t\t\t<== Add ICU ==>\n\n");

    fp = fopen("icu.txt", "ab");


    printf("\t\t\t\t\t\t\tEnter Hospital Name: ");
    fflush(stdin);
    gets(icu.name);
    printf("\t\t\t\t\t\t\tEnter Hospital Address: ");
    fflush(stdin);
    gets(icu.address);

    printf("\t\t\t\t\t\t\tEnter Free ICU bed: ");
    scanf("%d", &icu.ICU);
      setColor(2);
    printf("\t\t\t\t\t\t\tInformation Updated Successfully\n\n");

    fwrite(&icu, sizeof(icu), 1, fp);
    fclose(fp);
}



void ICUList()
{
    setColor(235);
    system("cls");
    printf("\n\n\t\t\t\t\t\t\t\t<== ICU List ==>\n\n");

    printf(" %-30s %-30s %-10s  %s\n", "Name", "Address", " ICU","Date");
    printf("-------------------------------------------------------------------------------------------------------------------\n");

    fp = fopen("ICU.txt", "rb");
    while(fread(&icu, sizeof(icu), 1, fp) == 1)
    {
        printf(" %-30s %-30s %-10d %s\n", icu.name, icu.address, icu.ICU, icu.date);
    }

    fclose(fp);
}
void deleteICU()
{
    char name[50];
    int f = 0;
    system("cls");
    printf("\n\n\t\t\t\t\t\t<== Delete ICU ==>\n\n");
    printf("\t\t\t\t\t\tEnter ICU Name: ");
    scanf("%s", name);

    FILE *fp;
    FILE *ft;

    fp = fopen("ICU.txt", "rb");
    ft = fopen("temp.txt", "wb");

    if (fp == NULL || ft == NULL)
    {
          setColor(4);
        printf("\n\n\t\t\t\t\t\tError opening file!");
        return;
    }

    while (fread(&icu, sizeof(icu), 1, fp) == 1)
    {
        if (strcmp(name, icu.name) == 0)
        {
            f = 1;
        }
        else
        {
            fwrite(&icu, sizeof(icu), 1, ft);
        }
    }

    if (f == 1)
    {
          setColor(2);
        printf("\n\n\t\t\t\t\t\tDelete ICU  Successfully.");
    }
    else
    {
          setColor(4);
        printf("\n\n\t\t\t\t\t\tRecord Not Found!");
    }

    fclose(fp);
    fclose(ft);

    remove("ICU.txt");
    rename("temp.txt", "ICU.txt");
}
void setColor(int ForgC);
void gotoxy(int x, int y);

void aboutus()
{

    system("cls");

    setColor(236);

    printf("\n\n\n\t\t\t\t\t\t Hospital Management System");
    printf("\n\t\t\t\t\t\t --------------------------\n");


    printf("\n\n\t\t\t\t\t\t By Team Backbenchers");

    setColor(238);


    printf("\n\n\t\t\t\t\t\t Developed By:");

    setColor(235);

    printf("\n\t\t\t\t\t\t MD Mejbah Uddin");
    printf("\n\t\t\t\t\t\t Munyim Mahmud");
    printf("\n\t\t\t\t\t\t Sajedul Alam Fahim");
    printf("\n\t\t\t\t\t\t Mohammed Hasibul islam");

    setColor(238);
    printf("\n\n\t\t\t\t\t\t Pursuing B.Sc in CSE");

    printf("\n\t\t\t\t\t\t from IIUC\n");
    setColor(9);
    printf(" \n\t\t\t\t\t\t CONTACTS : hasibulislam2003132@gmail.com");

}

void setColor(int ForgC)
{
    HANDLE hStdOut = GetStdHandle(STD_OUTPUT_HANDLE);

    CONSOLE_SCREEN_BUFFER_INFO csbi;

    if (GetConsoleScreenBufferInfo(hStdOut, &csbi))
    {
        WORD wColor = (csbi.wAttributes & 0xF0) + (ForgC & 0x0F);
        SetConsoleTextAttribute(hStdOut, wColor);
    }
}

void gotoxy(int x, int y)
{
    COORD coord;
    coord.X = x;
    coord.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}
