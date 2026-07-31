//i just leaned to code using windows.h API and made a simple movement game just press W,A,S or D key on ur keyboard to play it
#include<iostream>
#include<conio.h> // for _kbhit() and getch()
#include<windows.h> //for sleep() and drawing shapes
using namespace std;
//Global declarations
bool is_running = true;

//Global functions declarations
void SetCursor(int x, int y) //place cursor position to (x,y) cords
{
    COORD coord = {short(x), short(y)};
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}
void CreateBox(int x, int y, int width, int height) //create a box from (x,y) cords up to a area
{
    for (int row = 0; row < height; row++)
    {
        SetCursor(x, (y + row));
        system("Color 0C");
        for(int col = 0; col < width; col++)
        {
            std::cout << "#";
        }
    }
}
void EraseBox(int x, int y, int width, int height) // erase a box starting from (x,y) up to a area
{
    for (int row = 0; row < height; row++)
    {
        SetCursor(x, y + row);
        for(int col = 0; col < width; col++)
        {
            std::cout << " ";
        }
    }
}

// creating a player class
class Player
{
public:
    //coordinates of the player
    int x_cord;
    int y_cord;
    int score;
    int speed;
    char direction;

    void Direction_and_Exit() //for movement of player and exit of game
    {
        if (_kbhit())
        {

            switch(_getch())
            {

            case 'w':
                direction = 'u';
                break;

            case 'a':
                direction = 'l';
                break;

            case 's':
                direction = 'd';
                break;

            case 'd':
                direction = 'r';
                break;

            case 'x':
                is_running = false;
                break;
            }
        }
        switch(direction)
        {
        case 'u':
            y_cord--;
            break;

        case 'd':
            y_cord++;
            break;

        case 'l':
            x_cord--;
            break;

        case 'r':
            x_cord++;
            break;
        }
    }

};

//creating a fruit class
class fruit
{
public:
    int xCord;
    int yCord;
};

//main program
int main()
{
    //variables declarations
    int x = 25, y = 5;
    int height = 20, width = 50;

    Player WTF_A_CIRCLE;
    WTF_A_CIRCLE.x_cord = x + (width / 2);
    WTF_A_CIRCLE.y_cord = y + (height / 2);
    WTF_A_CIRCLE.score = 0;
    WTF_A_CIRCLE.speed = 300; //adjust speed by refresh time

    fruit JUST_A_F;
    JUST_A_F.xCord = x + (rand() % (width - 2) + 1);
    JUST_A_F.yCord = y + (rand() % (height - 2) + 1);

    //title screen
    std::cout<< "======================================================================================================================"<< std::endl;
    std::cout<< "                                                            Snake Game                                                "<< std::endl;
    std::cout<< "======================================================================================================================"<< std::endl;
    std::cout<< "\n What would you like to do?? \n 1.Start Game(Pres any key) \n 2.Exit Game(Press 'X')";

    if (_getch() == 'x')
        return 0;

    system("cls");
    std::cout << "Press [W], [A], [S], [D] to change direction and [X] to exit";

    //make a boundary box
    CreateBox(x, y, width, height);
    EraseBox(x + 1, y + 1, width - 2, height - 2);

    while (is_running)
    {

        SetCursor( WTF_A_CIRCLE.x_cord,  WTF_A_CIRCLE.y_cord);
        system("Color 0A");  //set color to green
        std::cout << "O";

        //print fruit
        SetCursor(JUST_A_F.xCord, JUST_A_F.yCord);
        std::cout << "F";

        SetCursor(1, y + height + 3);
        std::cout <<"Player score: " << WTF_A_CIRCLE.score;

        //for player(circle) movement
        WTF_A_CIRCLE.Direction_and_Exit();

        //collision detection
        if ( WTF_A_CIRCLE.x_cord == x ||  WTF_A_CIRCLE.y_cord == y ||  WTF_A_CIRCLE.x_cord == (x + width - 1) ||  WTF_A_CIRCLE.y_cord == (y + height - 1))
        {
            is_running = false;
            continue;
        }
        //fruit collision detection
        if (JUST_A_F.xCord == WTF_A_CIRCLE.x_cord && JUST_A_F.yCord == WTF_A_CIRCLE.y_cord)
        {
            JUST_A_F.xCord = x + (rand() % (width - 2) + 1);
            JUST_A_F.yCord = y + (rand() % (height - 2) + 1);
            WTF_A_CIRCLE.score++;  //increment score
            EraseBox(14, y + height + 3, 10, 1);  //update score board


            if (WTF_A_CIRCLE.score % 10 == 0 && WTF_A_CIRCLE.speed >= 50)  //every 10 fruit eaten
            {
                WTF_A_CIRCLE.speed -= 25;  //increase speed of circle
            }
        }

        Sleep(WTF_A_CIRCLE.speed); //stops before refreshing

        //Erase the area inside box
        EraseBox(x + 1, y + 1, width - 2, height - 2);
    }

    SetCursor(1, y + height + 4);
    system("Color 0F"); //change color to white
    std::cout << "GAME OVER!!!" << std::endl;
    Sleep(1000);
    return 0;
}
//Just tail logic is remaining.....
