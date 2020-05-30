# School-Event-Manager-linked-list-
School Event Manager For the purpose of practicing with linked-list
/* * * * * * * * * * * * * * * * * 
 * Name: Hanibal Alazar          *
 * Project: list program         *
 * Class:CSE 1002 section 03     *
 * line: 132                     *
 * * * * * * * * * * * * * * * * */
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct{
    int hour;
    int mintue;
}event_time_t;

//this is the struct for date
typedef struct{
    int month;
    int day;
    int year;
}event_date_t;

//this is the struct for a single event
typedef struct node_s{
    char event_title[20];
    event_time_t * time;
    event_date_t * date;
    struct node_s * next;
}event_t;

event_t * add_events(event_t* head_list){
    event_t * temp_ptr;// Declare 

    while(!feof(stdin)){
        /*
            Allocate memory for the EVENT strut and the two structs that make it up
                TIME 
                DATE
                Using two new pointers 
        */
        event_t * new_event_node = (event_t*)malloc(sizeof(event_t));
        event_time_t *time_event = (event_time_t*)malloc(sizeof(event_time_t));
        event_date_t *date_event = (event_date_t*)malloc(sizeof(event_date_t));

        /*This is an an Error check block where if any of the allocated 
            equal to NULL then it was not allocared correctly
        */
        if(new_event_node ==NULL){
            printf("Error Incorect Memory Allocation For time_t struture");
            if(time_event ==NULL){
                printf("Error Incorect Memory Allocation For date_t struture");
                if(date_event ==NULL)
                    printf("Error Incorect Memory Allocation For event_t struture");
                    exit(0);
            }
        }
        new_event_node->time = time_event; //This links the new pointer to fields of
        new_event_node->date = date_event; //of each element within the stucts

        //Input for the new node using the two pointer allocated early 
        scanf("%s",new_event_node->event_title);
        scanf("%d%d",&time_event->hour,&time_event->mintue);
        scanf("%d%d%d", &date_event->month, &date_event->day, &date_event->year);

        new_event_node->next =NULL;
        temp_ptr = head_list;

        if(head_list == NULL ||//checks if there is a links list to begin with
            strcmp(new_event_node->event_title, head_list->event_title) <=0 ){
            //the new node becomes the new head_node
            new_event_node->next = head_list; 
            head_list = new_event_node;
        }

        else{
            while(temp_ptr->next != NULL &&
              strcmp(new_event_node->event_title, temp_ptr->next->event_title) > 0){
                temp_ptr = temp_ptr->next;
            }
            //goes through the entore list find the correct postion for the new node
            new_event_node->next = temp_ptr->next;
            temp_ptr->next = new_event_node;
        }
    }
    /*This assigns the list created to head file so that 
       it can be return to main and accessed by other function 
    */
    head_list = temp_ptr;
    return (head_list);
}
void print_list(event_t *header_node){
    event_t * temp_ptr = header_node;
    //goes the entire list and prints the element fields in a struture method
    while (temp_ptr != NULL) {
        printf("\t%s \t",temp_ptr->event_title);// \t charecter is used fot the output format required
        printf("at: %.2d:%.2d\ton: ",temp_ptr->time->hour, temp_ptr->time->mintue); 
        printf("%.2d/%.2d/%.2d \n",temp_ptr->date->month, temp_ptr->date->day,temp_ptr->date->year);
      
        temp_ptr = temp_ptr->next;
    }
}
void print_selected_events(event_t * head_node, event_date_t date){
    event_t *temp_ptr = head_node;
    while (temp_ptr != NULL){
        //prints the field of every element with specific date
        if(date.month == temp_ptr->date->month && date.day == temp_ptr->date->day && date.year == temp_ptr->date->year ){
            printf("\t%s \t",temp_ptr->event_title);// \t charecter is used fot the output format required
            printf("at: %.2d:%.2d\n",temp_ptr->time->hour, temp_ptr->time->mintue); 
        }
        temp_ptr = temp_ptr->next;
    }
}
int main(void){
    event_t * head_node = NULL;
    // assign the values for the specfic date used in the print_selected_events
    // print_selected_events function 
    event_date_t event_date;
    event_date.month = 06; 
    event_date.day =15; 
    event_date.year = 2018;

    //assigns list created in add_events to head to be used in other functions 
    head_node = add_events(head_node);

    printf("\nSchedule of Events:\n");
    print_list(head_node);//to print the list 
    printf("\nDate:\t06/15/2018\nEvents\n");
    print_selected_events(head_node, event_date); //call print_selected_evnets
    printf("\n");

    return 21;
}
