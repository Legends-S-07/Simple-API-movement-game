//i just leaned to code using windows.h API and made a simple movement game just press W,A,S or D key on ur keyboard to play it
#include<iostream>
#include<conio.h> // for _kbhit() and getch()
#include<windows.h> //for sleep() and drawing shapes
//Global declarations
bool is_running = true;

//Global functions declarations
void goto_out(int x, int y) //place cursor position to (x,y) cords
{
    COORD coord = {short(x), short(y)};
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}
void CreateBox(int x, int y, int width, int height) //create a box from (x,y) cords up to a area
{
    for (int row = 0; row < height; row++)
    {
        goto_out(x , (y + row));
        system("Color 0C");
        for(int col = 0; col < width; col++){
            std::cout << "*";
        }
    }
}
void EraseBox(int x, int y, int width, int height) // erase a box starting from (x,y) up to a area
{
    for (int row = 0; row < height; row++)
    {
        goto_out(x , y + row);
        for(int col = 0; col < width; col++){
            std::cout << " ";
        }
    }
}

// creating a player class
class Player{
public:
    //coordinates of the player
    int x_cord;
    int y_cord;

    void Movement_and_exit() //for movement of player and exit of game
    {
            switch(_getch())
            {

            case 'w':
                y_cord--;
                break;

            case 'a':
                x_cord--;
                break;

            case 's':
                y_cord++;
                break;

            case 'd':
                x_cord++;
                break;

            case 'x':
                is_running = false;
                break;
            }
        }
};

//main program
int main()
{
    //variables declarations
    int x = 25, y = 25;
    int height = 30, width = 50;
    Player WTF_A_CIRCLE;
    WTF_A_CIRCLE.x_cord = x + (width / 2);
    WTF_A_CIRCLE.y_cord = y + (height / 2);

    //make a boundary box
    CreateBox(x, y, width, height);
    EraseBox(x + 1, y + 1, width - 2, height - 2);

    while (is_running)
    {
        //collision detection
        if ( WTF_A_CIRCLE.x_cord == x ||  WTF_A_CIRCLE.y_cord == y ||  WTF_A_CIRCLE.x_cord == (x + width - 1) ||  WTF_A_CIRCLE.y_cord == (y + height - 1)){
            is_running = false;
            continue;
         }
        goto_out( WTF_A_CIRCLE.x_cord,  WTF_A_CIRCLE.y_cord);
        system("Color 0A");  //set color to green
        std::cout << "O";
        Sleep(150); //adjust speed by refresh time

        //for player(circle) movement
        WTF_A_CIRCLE.Movement_and_exit();

        //Erase the area inside box
        EraseBox(x + 1, y + 1, width - 2, height - 2);
    }

    goto_out(1, y + height + 3);
    system("Color 0F"); //change color to white
    std::cout << "GAME OVER!!!" << std::endl;
    Sleep(1000);
    return 0;
}
//this is the complete code suggest if i can make any adjustments
