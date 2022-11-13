#include<windows.h>
#include<bits/stdc++.h>
#include<conio.h>
using namespace std;
long long score = 0;
int n;
void Setwindowsize(SHORT width, SHORT height){
   HANDLE hStdout=GetStdHandle(STD_OUTPUT_HANDLE);
   SMALL_RECT Windowsize;
   Windowsize. Top=0;
   Windowsize. Left=0;
   Windowsize. Right=width;
   Windowsize.Bottom=height;
   SetConsoleWindowInfo(hStdout, 1, &Windowsize);
}
void GoTo_xy(SHORT posX, SHORT posY)
{
	HANDLE hStdout = GetStdHandle(STD_OUTPUT_HANDLE);
    COORD Position;
    Position.X = posX;
    Position.Y = posY;

	SetConsoleCursorPosition(hStdout, Position);
}
void SetColor(int backgound_color, int text_color){
    HANDLE hStdout = GetStdHandle(STD_OUTPUT_HANDLE);

    int color_code = backgound_color * 16 + text_color;
    SetConsoleTextAttribute(hStdout, color_code);
//
//0 = Black      
//1 = Blue
//2 = Green 
//3 = Aqua
//4 = Red    
//5 = Purple    
//6 = Yellow
//7 = White   
//8 = Gray
//9 = Light Blue
//10 = Light Green
//11 = Light Aqua
//12 = Light Red
//13 = Light Purple
//14 = Light Yellow
//15 = Bright White
//
}
//
void Title(){
	printf("%70s\n"," [[||]    [[|]]        [[[|]      [[|]] ");
	SetColor(0,1); //color
	printf("%70s\n","[[[ ]|]  [[[ ]|]      [[[ |||    [[[ ]]]");
	SetColor(0,2); //color
	printf("%70s\n","    []]  ||| |||     [[[  |||    ||| |||");
	SetColor(0,3); //color
	printf("%70s\n","   []]   ||| |||    [[[   |||     [[O]] ");
	SetColor(0,4); //color
	printf("%70s\n","  []]    ||| |||   [[[    |||    ||| |||");
	SetColor(0,5); //color
	printf("%70s\n"," []]     [[[ ]]]  [[[||||||||||  [[[ ]]]");
	SetColor(0,6); //color
	printf("%70s\n","[][||||   [[|]]           |||     [[|]] ");
	SetColor(0,7);
}
void printWord(int x,int y,string c,int a,int b){
	GoTo_xy(x,y);
	SetColor(a,b);
	cout << c << endl;
	SetColor(0,7);
}
void Tuyn(){
	printWord(68,40,"  ",3,0);
	printWord(70,40,"Tuyn",0,3);
	printWord(78,40,"  ",3,0);
	printWord(68,41,"  ",3,0);
	printWord(70,41,"7/5/2022",0,3);
	printWord(78,41,"  ",3,0);
}
bool matrixzero(int a[][10]){
	for(int i = 0 ; i < n ; i++){
		for(int j = 0 ; j < n ; j++){
			if(a[i][j] == 0) return true;
		}
	}
	return false;
}
void random(int a[10][10]){
	srand(time(NULL)); 
	int k = rand() % (n-1 - 0 + 1) + 0,l = rand() % (n-1 - 0 + 1) + 0;
	if(matrixzero(a))
		while(1<2){
			if(a[k][l] == 0){a[k][l] = 2; break;}
			else{
				k = rand() % (n-1 - 0 + 1) + 0,l = rand() % (n-1 - 0 + 1) + 0;
			}
		}
	else return;
}
void inmatran(int a[10][10]){	
	for(int i = 0 ; i < n ; i++){
		GoTo_xy(30-n,20+i);
		cout << "|";
		for(int j = 0 ; j < n ; j++){
			if(a[i][j] != 0) SetColor(14,0);
			else SetColor(0,7);
			printf("%5d",a[i][j]);
			printf("     ");
			SetColor(0,7);
			printf("|");
		}
		cout << endl;
	}
	GoTo_xy(68,45);
	cout << "Point: ";
	SetColor(4,0);
	cout << "   " <<score<< "   " << endl;
	Tuyn();
}
bool check(int a[10][10]){
	for(int i = 0 ; i < n ; i ++){
		for(int j = 0 ; j < n ; j ++){
			if(a[i][j-1] == a[i][j] || a[i+1][j] == a[i][j] || a[i][j+1] == a[i][j] || a[i-1][j] == a[i][j]) return true;
		}
	}
	return false;
}
void up(int a[10][10],int &count){	
	// Chuyen so len dau
	for(int i = 1 ; i < n ; i++){
		for(int j = 0 ; j < n ; j++){
			if(a[i][j] != 0){
				int l = j,k = 0;
				while(k < i){
					if(a[k][l] == 0){
						a[k][l] = a[i][j];
						a[i][j] = 0;
						break;
					}
					k+=1;
				}
			}
		}
	}
	// Cong + vao 
	for(int i = 0 ; i < n-1 ; i++){
		for(int j = 0 ; j < n ; j++){
			if(a[i][j] == a[i+1][j]){
				a[i][j] += a[i+1][j];
				score += a[i+1][j];
				a[i+1][j] = 0;
				count+=1;
				continue;
			}
			if(a[i][j] == 0){
				a[i][j]+=a[i+1][j];
				a[i+1][j] = 0;
				count+=1;
			}
		}
	}
	random(a);
	
}
void down(int a[10][10],int &count){
	// Chuyen so xuong duoi
	for(int i = n-2 ; i >=0 ; i--){
		for(int j = 0 ; j < n ; j++){
			if(a[i][j] != 0){
				int l = j,k = n-1;
				while(k > i){
					if(a[k][l] == 0){
						a[k][l] = a[i][j];
						a[i][j] = 0;
						break;
					}
					k-=1;
				}
			}
		}
	}
	// Cong + vao 
	for(int i = n-1 ; i > 0 ; i--){
		for(int j = 0 ; j < n ; j++){
			if(a[i][j] == a[i-1][j]){
				a[i][j] += a[i-1][j];
				a[i-1][j] = 0;
				count+=1;
				continue;
			}
			if(a[i][j] == 0){
				a[i][j]+=a[i-1][j];
				a[i-1][j] = 0;
				count+=1;
			}
		}
	}
	random(a);
}
void left(int a[10][10],int &count){
	// Chuyen so sang trai
	for(int j = 1 ; j < n ; j++){
		for(int i = 0 ; i < n ; i++){
			if(a[i][j] != 0){
				int l = 0,k = i;
				while(l < j){
					if(a[k][l] == 0){
						a[k][l] = a[i][j];
						a[i][j] = 0;
						break;
					}
					l+=1;
				}
			}
		}
	}

	// Cong + vao 
	for(int j = 0 ; j < n-1 ; j++){
		for(int i = 0 ; i < n ; i++){
			if(a[i][j] == a[i][j+1]){
				a[i][j] += a[i][j+1];
				a[i][j+1] = 0;
				count+=1;
				continue;
			}
			if(a[i][j] == 0){
				a[i][j]+=a[i][j+1];
				a[i][j+1] = 0;
				count+=1;
			}
		}
	}
	random(a);
}
void right(int a[10][10],int &count){
	// Chuyen so sang phai
	for(int j = n-2 ; j >= 0 ; j--){
		for(int i = 0 ; i < n ; i++){
			if(a[i][j] != 0){
				int l = n-1,k = i;
				while(l > j){
					if(a[k][l] == 0){
						a[k][l] = a[i][j];
						a[i][j] = 0;
						break;
					}
					l-=1;
				}
			}
		}
	}
	// Cong + vao 
	for(int j = n-1 ; j > 0 ; j--){
		for(int i = 0 ; i < n ; i++){
			if(a[i][j] == a[i][j-1]){
				a[i][j] += a[i][j-1];
				a[i][j-1] = 0;
				count+=1;
				continue;
			}
			if(a[i][j] == 0){
				a[i][j]+=a[i][j-1];
				a[i][j-1] = 0;
				count+=1;
			}
		}
	}
	random(a);
}
void TestLose(){
	printWord(40,35,"  ",4,0);
	printWord(42,35,"Thua cmm roi",4,0);
	printWord(54,35,"  ",4,0);
}
void game(){
	GoTo_xy(35,20); cout << "Nhap do lon 2048: "; cin >> n;
	int a[10][10] = {0};
	random(a);
	int count  = -1;
	inmatran(a);
	for(;;){
		char c;
		c = getch();
		if(c == -32) c = getch();
		if(count == 0){
			if(check(a) == false){
				TestLose();
				return;
			}
		}
		count = 0;
		if(c == 72){
			up(a,count);
			inmatran(a);
		}else if(c == 80){
			down(a,count);
			inmatran(a);
		}else if(c == 75){
			left(a, count);
			inmatran(a);
		}else if(c == 77){
			right(a, count);
			inmatran(a);
		}else{
			inmatran(a);
		}
	}
	
}

int main(){
	Setwindowsize(100,50); //khung console
	Title();
	game();
	system("pause");
}
