#include<iostream> 
#include<deque>  
#include <fstream>  
#include <iomanip> //бібліотека, яка містить команду setw

using namespace std;//визначення простору імен

int n,//кількість вершин
m,//кількість ребер
a[100][100],//матриця ребер
s[100][100];//матриця суміжності
int top[100], //топологічне сортування
sizeofkomp = 0, sumofsize=0, multiplex=1;

void input(); //функція введення даних
void matrix(); //функція створення матриці суміжності
void sort(); //функція топологічного сортування
void komp(); //функція знаходження компонент сильної зв'язності

deque<int> myDeque; //двустороння черга
//====== Головна функція програми ========
int main()
{
	setlocale(LC_ALL, "Russian");
	input(); //функція введення даних
	matrix(); //функція створення матриці суміжності
	sort(); //функція топологічного сортування
	komp(); //функція знаходження компонент сильної зв'язності
	system("pause");
}
//====== Функція введення даних ========
void input()
{
	long IO;
	ifstream fin("in.txt"); //відкрити файл для читання
	fin >> n; //зчитати кількість вершин
	fin >> m; //зчитати кількість ребер
	fin >> IO;
	for (int i = 1; i <= m; i++)
	for (int j = 1; j <= 2; j++)
		fin >> a[i][j]; //зчитування початкових і кінцевих вершин ребра
	fin.close(); //закрити файл
}
//===== Функція створення матриці суміжності =====
void matrix()
{
	//==== Створення матриці суміжності ======
	for (int i = 1; i <= n; i++)
	for (int j = 1; j <= n; j++)
		s[i][j] = 0; //заповнення матриці суміжності нулями

	for (int i = 1; i <= m; i++)
	{
		s[a[i][1]][a[i][2]] = 1; //заповнити матрицю суміжності

	}

	cout << "Матрица смежности:" << endl;
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
			cout << s[i][j] << " ";
		cout << endl;
	} //виведення матриці суміжності
}
//===== Функція топологічного сортування =====
void sort()
{
	cout << endl;
	bool mas[100]; //масив пройдених вершин
	for (int i = 1; i <= n; i++)
		mas[i] = false; //всі вершини не пройдені
	int p, //вершина, з якої починається пошук
		DFS[100];
	//cin >> p; //зчитати початкову вершину
	p = 1;
	cout << "Вершина ------ #DFS ---- Queue" << endl;
	mas[p] = true; //вершина пройдена
	DFS[1] = p;
	myDeque.push_back(p); //додати вершину в стек
	cout << setw(4) << p << setw(14) << 1 << setw(8);
	for (int i = 0; i < myDeque.size(); i++)
		cout << myDeque[i] << " "; //вивести стек
	cout << endl;
	int size;//розмір черги
	int k = 1;
	int z = n;
	while (!myDeque.empty())
	{
		int v = myDeque.front();
		mas[v] = true;
		bool flag = 0;
		for (int j = 1; j <= n; j++)
		{
			if (mas[j] == false && s[v][j] == 1)
			{
				myDeque.push_front(j);
				mas[j] = true;
				k++;
				DFS[k] = j;
				cout << setw(4) << j << setw(14) << k << setw(8);
				for (int i = 0; i < myDeque.size(); i++)
					cout << myDeque[i] << " ";
				cout << endl;
				size = myDeque.size();
				flag = 1;
				break;

			}
			if (myDeque.size() == size - 1)
			{
				cout << setw(4) << "-" << setw(14) << "-" << setw(8);
				for (int i = 0; i < myDeque.size(); i++)
					cout << myDeque[i] << " ";
				cout << endl;
				size -= 1;
			}
		}
		if (flag == 0)
		{
			top[z] = v;
			z--;
			myDeque.pop_front();
		}
	}
	bool flag1 = 0;
	for (int j = 1; j <= n; j++)
	{
		if (mas[j] == false)
		{
			p = j;
			mas[p] = true; //вершина пройдена
			DFS[1] = p;
			myDeque.push_back(p); //додати вершину в стек
			cout << setw(4) << p << setw(14) << 1 << setw(8);
			for (int i = 0; i < myDeque.size(); i++)
				cout << myDeque[i] << " "; //вивести стек
			cout << endl;
			while (!myDeque.empty())
			{
				int v = myDeque.front();
				mas[v] = true;
				bool flag = 0;
				for (int j = 1; j <= n; j++)
				{
					if (mas[j] == false && s[v][j] == 1)
					{
						myDeque.push_front(j);
						mas[j] = true;
						k++;
						DFS[k] = j;
						cout << setw(4) << j << setw(14) << k << setw(8);
						for (int i = 0; i < myDeque.size(); i++)
							cout << myDeque[i] << " ";
						cout << endl;
						size = myDeque.size();
						flag = 1;
						break;
					}
					if (myDeque.size() == size - 1)
					{
						cout << setw(4) << "-" << setw(14) << "-" << setw(8);
						for (int i = 0; i < myDeque.size(); i++)
							cout << myDeque[i] << " ";
						cout << endl;
						size -= 1;
					}
				}
				if (flag == 0)
				{
					top[z] = v;
					z--;
					myDeque.pop_front();
				}
			}
		}
	}
	cout << "SORT: ";
	for (int i = 1; i <= n; i++)
		cout << top[i] << " ";
	cout << endl;
}
//===== Функція знаходження компонент сильної зв'язності ======
void komp()
{
	//==== Створення матриці суміжності ======
	for (int i = 1; i <= n; i++)
	for (int j = 1; j <= n; j++)
		s[i][j] = 0; //заповнення матриці суміжності нулями

	for (int i = 1; i <= m; i++)
		s[a[i][2]][a[i][1]] = 1;

	cout << "Матрица смежности транспонированного графа:" << endl;
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
			cout << s[i][j] << " ";
		cout << endl;
	} //виведення матриці суміжності
	bool mas[100]; //масив пройдених вершин
	for (int i = 1; i <= n; i++)
		mas[i] = false; //всі вершини не пройдені
	int kompNumber = 0;//кількість компонент сильної зв'язності
	for (int i = 1; i <= n; i++)
	{
		int p; //вершина, з якої починається пошук
		int DFS[100];
		if (mas[top[i]] == false)
		{
			kompNumber++;
			p = top[i];
			mas[p] = true; //вершина пройдена
			DFS[1] = p;
			myDeque.push_back(p); //додати вершину в стек
			int size;//розмір черги
			int k = 1;
			int z = n;
			while (!myDeque.empty())
			{
				int v = myDeque.front();
				mas[v] = true;
				bool flag = 0;
				for (int j = 1; j <= n; j++)
				{
					if (mas[j] == false && s[v][j] == 1)
					{
						myDeque.push_front(j);
						mas[j] = true;
						k++;
						DFS[k] = j;
						size = myDeque.size();
						flag = 1;
						break;

					}
				}
				if (flag == 0)
				{
					myDeque.pop_front();
				}
			}
			cout << "Компонента связности: ";
			for (int i = 1; i <= k; i++)
			{

				cout << DFS[i] << " ";
				sizeofkomp++;
			}
			cout << endl;
			sumofsize += sizeofkomp;
			multiplex = multiplex*sizeofkomp;
			sizeofkomp = 0;
		}
	}
	cout << "Количество компонент сильной связности: " << kompNumber << endl;
	int answer;
	answer = multiplex*(pow(sumofsize, (kompNumber - 2)));
	cout << "\n Answer: " << answer<<"\n";
	ofstream fout("out.txt"); //відкрити файл для читання
	fout << answer;
	fout.close();
	system("START out.txt");
}
