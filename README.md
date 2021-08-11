//# parking-management
//This project shows the management of parking system.
#include<stdio.h>
#include<stdlib.h>
#define CAR 1
#define SCOOTER 2
struct vehicle
{
    int num ;
    int row ;
    int col ;
    int type ;
    int a, b;
	float r;
} ;
int parkinfo[4][10] ; 
int vehcount ;  
int carcount ;    
int scootercount ;  
void display( ) ;
struct vehicle * add ( int, int, int, int ) ;
struct vehicle * del ( int, int, int, int ) ;
void getfreerowcol ( int, int * ) ;
void getrcbyinfo ( int, int, int * ) ;
void show( ) ;
float rate(char,int,int);
/* adds a data of vehicle */
struct vehicle * add ( int t, int num, int row, int col )
{
    struct vehicle *v ;
    v = ( struct vehicle * ) malloc ( sizeof ( struct vehicle ) ) ;
    v -> type = t ;
    v -> row = row ;
    v -> col = col ;
    if ( t == CAR )
           carcount++ ;
    else
        scootercount++ ;
    vehcount++ ;
    parkinfo[row][col] = num ;
    return v ;
}

/* get the row-col position for the vehicle with specified number */
void getrcbyinfo ( int type, int num, int *arr )
{
    int r, c, fromrow = 0, torow = 2 ;
    if ( type == SCOOTER )
    {
        fromrow += 2 ;
        torow += 2 ;
    }
    for ( r = fromrow ; r < torow ; r++ )
    {
        for ( c = 0 ; c < 10 ; c++ )
        {
            if ( parkinfo[r][c] == num )
            {
                arr[0] = r ;
                arr[1] = c ;
                return ;
            }
        }
    }
    if ( r == 2 || r == 4 )
    {
        arr[0] = -1 ;
        arr[1] = -1 ;
    }
}
/* get the row-col position for the vehicle to be parked */
void getfreerowcol ( int type, int *arr )
{
    int r, c, fromrow = 0, torow = 2 ;
    if ( type == SCOOTER )
    {
        fromrow += 2 ;
        torow += 2 ;
    }
    for ( r = fromrow ; r < torow ; r++ )
    {
        for ( c = 0 ; c < 10 ; c++ )
        {
            if ( parkinfo[r][c] == 0 )
            {
                arr[0] = r ;
                arr[1] = c ;
                return ;
            }
        }
    }
    if ( r == 2 || r == 4 )
    {
        arr[0] = -1 ;
        arr[1] = -1 ;
    }
}
/* displays list of vehicles parked */
void display( )
{
    int r, c ;    
            printf ( "		     !!!###__CARS__###!!!		\n" ) ;
            printf("******************************************************************************\n");
    for ( r = 0 ; r < 4 ; r++ )
    {
        if ( r == 2 )
            printf ( "\n     	  	   !!!###__SCOOTERS__###!!!		\n" ) ;
            printf("******************************************************************************\n");
        for ( c = 0 ; c < 10 ; c++ )
           printf ( "%d\t", parkinfo[r][c] ) ;
           printf("\n******************************************************************************");
        printf ( "\n" ) ;
    }
}
float rate(char t,int x, int y)
{
    float rate = 0;
	switch (t)
	{
	case '1':if (y - x <= 2)
		{
		 rate = 30;
		 break;
		}
		 else
		{
		    rate = ((y - x)*20.50);
		    break;
	}
	case'2' : if (y - x <= 2)
		{
		    rate = 30;
		    break;
		}
		 else
		 {
			 rate = ((y - x)*10.50);
			 break;
		}
	}
	return rate;
}

int main( )
{
	system("color 3");
    int choice, type, number, row = 0, col = 0 ;
    int i, tarr[2] ;
    int finish = 1 ;
    char name[20];
    struct vehicle *v ;
    /* creates a 2-D array of car and scooter class */
struct vehicle *car[2][10] ;
    struct vehicle *scooter[2][10] ;
    /* displays menu and calls corresponding functions */
while ( finish )
    {
    	printf(" 	*************************************************\n");
        printf("	*************************************************");    	
        printf ( "\n	**           VEHICLE PARKING                   **\n" ) ;
        printf(" 	**=============================================**\n");
        printf ( " 	**	1. ARRIVAL OF VEHICLES		       **\n" ) ;
        printf ( " 	**	2. TOTAL NO. OF VEHICLES PARKED        **\n" ) ;
        printf ( " 	**	3. TOTAL NO. OF CARS PARKED            **\n" ) ;
        printf ( " 	**	4. TOTAL NO. OF SCOOTER PARKED         **\n" ) ;
        printf ( " 	**	5. DISPLAY PARKING                     **\n" ) ;
		printf ( " 	**	6. INVOICE                             **\n" ) ;
		printf ( " 	**	7. EXIT                                **\n" ) ;
        printf ( " 	**	                                       **\n" ) ;
        printf("	*************************************************\n");
        printf("	*************************************************\n");
        scanf ( "%d", &choice ) ;
        switch ( choice )
        {
            case  1 :
                type = 0 ;
                /* check for vehicle type */
while ( type != CAR && type != SCOOTER )
                {
                	printf ( "	*************************************************\n" ) ;
                    printf ( "	*ENTER VEHICLE TYPE (1 for CAR / 2 for SCOOTER):*\n" ) ;
                    printf ( "	*************************************************\n" ) ;
                    scanf ( "	%d", &type ) ;
                    if ( type != CAR && type != SCOOTER )
                    printf ( "              !!!SORRRRY!!!PLEASE SELECT VALID TYPE\n" ) ;
                }
                printf ( "	<<<ENTER VEHICLE NUMBER>>> \n" ) ;
                scanf ( "	%d", &number ) ;
                /* add cars' data */
if ( type == CAR || type == SCOOTER )
                {
                    getfreerowcol ( type, tarr ) ;
                    if ( tarr[0] != -1 && tarr[1] != -1 )
                    {
                        row = tarr[0] ;
                        col = tarr[1] ;
                        if ( type == CAR )
                            car[row][col] =  add ( type, number, row, col ) ;
                        else
                            scooter[row - 2][col] = add ( type, number, row, col ) ; ;
                    }
                    else
                    {
                        if ( type == CAR )
                             printf ( "\n!!PARKING FULL!! No More Cars\n" ) ;
                        else
                            printf ( "\n!!PARKING FULL!! No More scooters\n" ) ;
                    }
                }
                else
                {
                	 printf ( "!!!SORRRRY!!!PLEASE INSERT VALID INPUT\n" ) ;
                    break ;
                }
                printf ( "	PRESS ANY KEY TO CONTINUE...\n" ) ;
                break ;
            case  2 :
            	printf ( "		###################################\n" ) ;
                printf ( "		    TOTAL VEHICLES PARKED=%d\n", vehcount ) ;
                printf ( "		###################################\n" ) ;
                break ;
            case  3 :
            	printf ( "		###################################\n" ) ;
                printf ( "	            TOTAL CARS PARKED: %d\n", carcount ) ;
                printf ( "		###################################\n" ) ;
                break ;
            case  4 :
            	printf ( "		###################################\n" ) ;
                printf ( "		    TOTAL SCOOTERS PARKED: %d\n", scootercount ) ;
                printf ( "		###################################\n" ) ;
                break ;
            case  5 :
            	printf("              	!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!\n");
                printf ( "            	        PARKING STRUCTURE            \n" ) ;
                printf("              	!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!\n");
                printf("******************************************************************************\n");
                display( ) ;
                break ;
         case  6 :
                  int a, b;
	              float r;
	              char type;
	    printf(" 	*****************************************************\n");          
		printf("	ENTER THE TYPE OF VEHICLE (1 for car, 2 for scooter):\n ");
		printf(" 	*****************************************************\n");			
	                scanf("%s",&type);
	                    printf("\n");
		if (type == '1' || type == '1' || type == '2' || type == '2')
		{
    	printf(" 	*************************************************\n");			
    	printf("	ENTER THE TIME VEHICLE ENTRED ( from 0 to 24):\n");
		printf(" 	*************************************************\n");			
		scanf("		%d",&a);	
			printf("\n");
		printf(" 	***************************************************\n");
		printf("	ENTER THE TIME VEHICLE DEPARTURED ( from 0 to 24):\n");
		printf(" 	***************************************************\n");
		scanf("		%d", &b);
			printf("\n");
			r = rate(type, a, b);
		printf(" 	******************************\n");
		printf("	PLEASE PAY %.2f RUPEES\n", r);
		printf(" 	******************************\n");
			break;
		}
	return 0;
	case 7:exit;
            }
        }
    }




