#include <stdio.h>
#include <malloc.h>
#include <string.h>
#include <ctype.h>
/*----------------------------------------------------------------------------*/
/* Define Functions*/
void addNewrecord(void);
void listAll(void);
void modifyrecord(void);
void deleterecord(void);
int prompt(void);
int findnum (int recordnum);
/*----------------------------------------------------------------------------*/
/* Define Structures*/
typedef struct record {
 int number;
 char id[20];
 char name[20];
 char salary[20];
 struct record *next;
 int count;
 } Record;
Record *firstc,*currentc,*newc;
int cnum = 0;
/*----------------------------------------------------------------------------*/
/* Main Function */
int main()
{
 FILE *datafile;
 char *filename = "recorddatabase.dat";
 char ch;
 firstc = NULL;
 datafile = fopen(filename,"r");
 if(datafile)
 {
 firstc = (struct record *)malloc(sizeof(struct record));
 currentc = firstc;
 while(1)
 {
 newc = (struct record *)malloc(sizeof(struct record));
 fread(currentc,sizeof(struct record),1,datafile);
 if(currentc->next == NULL)
 break;
 currentc->next = newc;
 currentc->count=0;
 currentc = newc;
 }
 fclose(datafile);
 cnum = currentc->number;
 }
 do
 {
fflush(stdin);
 puts("1 - Create a record");
puts("2 - Display record");
puts("3 - Modify record");
puts("4 - Delete record");
puts("-- -----------------------------");
 puts("Q - Save and quit\n");
 printf("\tYour choice:");
 ch = getchar();
 ch = toupper(ch);

 switch(ch)
 {
 case '1':
 puts("Add a new record\n");
 fflush(stdin);
 addNewrecord();
 break;
 case '2':
puts("List all record\n");
listAll();
break;
 case '3':
puts("Modify a record\n");
modifyrecord();
break;
case '4':
puts("Delete a record\n");
 deleterecord();
 break;
 case 'Q':
 puts("Save and quit\n");
 default:
 break;
 }
 }
 while(ch != 'Q');
/*
* Save the records to disk
*/
 currentc = firstc;
 if(currentc == NULL)
 return(0);
 datafile = fopen(filename,"w");
 if(datafile == NULL)
 {
 printf("Error writing to %s\n",filename);
 return(1);
 }
 while(currentc != NULL)
 {
 fwrite(currentc,sizeof(struct record),1,datafile);
 currentc = currentc->next;
 }

 fclose(datafile);
 return(0);
}
/*----------------------------------------------------------------------------*/
void addNewrecord(void) /* add new record function*/
{
 newc = (struct record *)malloc(sizeof(struct record));
 if(firstc==NULL)
 firstc = currentc = newc;
 else
 {
 currentc = firstc;
 while(currentc->next != NULL)currentc = currentc->next;
 currentc->next = newc;
 currentc = newc;
 }
/* update the structure */
 cnum++;
 printf("%27s: %5i\n","Record number",cnum);
 currentc->number = cnum;
 printf("%27s: ","Enter employee name");


 gets(currentc->name);
 printf("%27s: ","Enter employee ID");
 gets(currentc->id);
 printf("%27s: ","Enter the salary");
 gets(currentc->salary);
 printf("record added!\n");
 currentc->count=0;
 currentc->next = NULL;
}
/*----------------------------------------------------------------------------*/
void listAll(void) /* list all records */
{
 if(firstc==NULL)
 puts("There are no records to display!");
 else
 {
 printf("%6s %-20s %-15s %-15s\n","Record#","Name","ID","Salary");
 puts("------ -------------------- ------------- -------------------");
 currentc=firstc;
 do
 {
 printf("%6d: %-20s %-15s %-20s\n",\
 currentc->number,\
 currentc->name,\
 currentc->id,\
 currentc->salary);
 }
 while((currentc=currentc->next) != NULL);
 }
}
void modifyrecord(void) /*modify record function*/

{
 int record, result;
 if(firstc==NULL)
 {
 puts("There are no records to modify!");
return;
 }
 listAll();
 printf("Enter record number to modify or change: ");
 scanf("%d",&record);
 result = findnum(record);
 if( result >0 ){
 printf("Record %d:\n",currentc->number);
 printf("Name: %s\n",currentc->name);
 if(prompt())
 gets(currentc->name);
 printf("ID: %s\n",currentc->id);
 if(prompt())
 gets(currentc->id);
 printf("Salary: %s\n",currentc->salary);
 if(prompt())
 gets(currentc->salary);
 return;
}
 printf("Record %d was not found!\n",record);
}
/*----------------------------------------------------------------------------*/
int prompt(void)
{
char ch;
 fflush(stdin);
printf("Update? (Y to update any other key to not)");
ch = getchar();


ch = toupper(ch);
fflush(stdin);
if(ch == 'Y')
{
printf("Enter new value: ");
return(1);
}
else
return(0);
}
int findnum (int recordnum)
{
 int record;
 record = recordnum;
 currentc = firstc;
 while(currentc != NULL)
 {
 if(currentc->number == record)


 {
 return 1;
 }
else
{
 currentc = currentc->next;
}
 }
 return -1;
}


void deleterecord(void) /*delete record function */
{
int record;
 struct record *previousa;
if(firstc==NULL)
{
  puts("There are no records to delete!");
   return;
}
 listAll();
printf("Enter record number to delete: ");
 scanf("%d",&record);
currentc = firstc;
while(currentc != NULL)
    {
if(currentc->number == record)
 {
if(currentc == firstc)
firstc=currentc->next;
else
previousa->next = currentc->next;
free(currentc);
printf("record %d deleted!\n",record);
printf("----------------------------------------\n");
return;
}
else
    {
previousa = currentc;
 currentc = currentc->next;
}
 }
  printf("record %d not found!\n",record);
   }

