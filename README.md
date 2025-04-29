# prog4-unti
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define COUNTOF(array) (sizeof(array) / sizeof(array[0]))
#pragma warning(disable:4996)
#define N 128
#define D 10000

typedef struct {
    char school_id[21];
    int prefecture_num;
    int separate;
    char school_name[128];
    char address[128];
}highschool_list;

void qsort(void *base, size_t number, size_t width,
int (*compare)(const void *,const void *)
);

//比較①
int compareid(const void* prt1, const void* prt2) {
    int n=strcmp(((highschool_list*)prt1)->school_id,((highschool_list*)prt2)->school_id);
    if(n<0)return -1;
    else if (n>0)return 1;
    return 0;
}

int comparename(const void* prt1, const void* prt2){
    int n=strcmp(((highschool_list*)prt1)->school_name,((highschool_list*)prt2)->school_name);
    if(n<0)return -1;
    else if(n>0)return 1;
    return 0;
}

int compareaddress(const void* prt1, const void* prt2){
    int n=strcmp(((highschool_list*)prt1)->address,((highschool_list*)prt2)->address);
    if(n<0)return -1;
    else if(n>0)return 1;
    return 0;
}

//比較②
int recompareid(const void* prt1, const void* prt2){
    int n = strcmp(((highschool_list*)prt1)->school_id,((highschool_list*)prt2)->school_id);
    if(n<0)return 1;
    else if (n>0)return -1;
    return 0;
}

int recomparename(const void* prt1, const void* prt2){
    int n = strcmp(((highschool_list*)prt1)->school_name,((highschool_list*)prt2)->school_name);
    if(n<0)return 1;
    else if(n>0)return -1;
    return 0;
}

int recompareaddress(const void* prt1, const void* prt2){
    int n = strcmp(((highschool_list*)prt1)->address,((highschool_list*)prt2)->address);
    if(n<0)return 1;
    else if(n>0)return -1;
    return 0;
}

void printfile(FILE* fpp,highschool_list data[], int count){
    for(int j = 0; j < count; j++) {
        fprintf(fpp, "%s %d %d %s %s\n",data[j].school_id, data[j].prefecture_num, data[j].separate, data[j].school_name, data[j].address);
    }
}

int main(int argc,char* argv[])
{
    FILE* fp;
    FILE* fpp;
    char *mode =argv[1];
    char *target_csv=argv[2];
    char *outputfile=argv[3];
    fp=fopen(target_csv,"r");
    if(fp==NULL){
        printf("file not open\n");
        return 1;
    }
    highschool_list data[D];
    char line[1024];
    int count = 0;
    while (fgets(line, sizeof(line),fp) != NULL){
     if(sscanf(line, "%[^,],%d,%d,%[^,],%[^\n]",
        data[count].school_id,
        &data[count].prefecture_num,
        &data[count].separate,
        data[count].school_name,
        data[count].address) == 5) {
        count++;
        }
        else {
            printf("format error: %s\n", line);
        }
    }
    fclose(fp);
    if(strcmp(mode,"work2")==0){
        fpp=fopen(outputfile,"w");
        if(fpp==NULL){
            printf("not found write error\n");
            return 1;
        }
        printfile(fpp, data,count);
    }
    if(strcmp(mode,"work3")==0){
        if(strcmp(outputfile,"IDup.txt")==0){
            qsort(data,count,sizeof(highschool_list),compareid);
        }
        else if(strcmp(outputfile,"IDdown.txt")==0){
            qsort(data,count,sizeof(highschool_list),recompareid);
        }
        else if(strcmp(outputfile,"NAMEup.txt")==0){
            qsort(data,count,sizeof(highschool_list),comparename);
        }
        else if(strcmp(outputfile,"NAMEdown.txt")==0){
            qsort(data,count,sizeof(highschool_list),recomparename);
        }
        else if(strcmp(outputfile,"ADDup.txt")==0){
            qsort(data,count,sizeof(highschool_list),compareaddress);
        }
        else if(strcmp(outputfile,"ADDdown.txt")==0){
            qsort(data,count,sizeof(highschool_list),recompareaddress);
        }
        else{
            printf("sort error\n");
            return 1;
        }
        fpp=fopen(outputfile,"w");
        printfile(fpp,data,count);
    }
    fclose(fpp);
    return 0;
}
