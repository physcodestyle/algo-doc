---
title: Задача Коммивояжёра
date: Last Modified
permalink: /article/TSP/index.html
eleventyNavigation:
    key: TSP
    order: 1
    title: Задача Коммивояжёра
---

### Область применения:

Алгоритм применим к графам и может использоваться по аналогии в современной жизни для поиска кратчайшего пути.

### Историческая справка и информация об авторах:

Точно неизвестно, когда задача коммивояжера впервые появилась. Однако, известна изданная в 1832 году книга с названием «Коммивояжер - как он должен вести себя и что должен делать для того, чтобы доставлять товар и иметь успех в своих делах - советы старого курьера», в которой описана задача, но математический аппарат для её решения не применяется. Зато в ней предложены примеры маршрутов для некоторых регионов Германии и Швейцарии.

Ранним вариантом задачи может рассматриваться задача Уильяма Гамильтона, которая заключалась в том, чтобы найти маршруты на графе с 20 узлами.

Первые упоминания в качестве математической задачи на оптимизацию принадлежат Карлу Менгеру, который сформулировал ее на математическом коллоквиуме в 1930 году так: «Мы называем задачей посыльного (поскольку этот вопрос возникает у каждого почтальона, в частности, её решают многие путешественники) задачу найти кратчайший путь между конечным множеством мест, расстояние между которыми известно.»

Вскоре появилось известное сейчас название - задача странствующего торговца, которую предложил Хасслер Уитни из Принстонского университета.

Вместе с простотой определения и сравнительной простотой нахождения хороших решений задача коммивояжёра отличается тем, что нахождение действительно оптимального пути является достаточно сложной задачей. Учитывая эти свойства, начиная со второй половины XX века исследование задачи коммивояжера имеет не столько практический смысл, сколько теоретический в качестве модели для разработки новых алгоритмов оптимизации.

Многие современные распространенные методы дискретной оптимизации, такие как метод отсечений, ветвей и границ и различные варианты эвристических алгоритмов, были разработаны на примере задачи коммивояжера.

В 1950-е и 1960-е годы задача коммивояжера привлекла внимание ученых в США и Европе. Важный вклад в исследование задачи принадлежит Джорджу Данцигу, Делберту Рею Фалкерсону и Селмеру Джонсону, которые в 1954 году в институте RAND Corporation сформулировали задачу в виде задачи дискретной оптимизации и применили для её решения метод отсечений. Используя этот метод, они построили путь коммивояжера для одной частной постановки задачи с 49 городами и обосновали его оптимальность.

### Идея (рисунки / анимации / видео):

Задача коммивояжера является одной из самых известных задач, заключающаяся в поиске самого выгодного маршрута, проходящего через указанные города хотя бы по одному разу с последующим возвратом в исходный город.
В условиях задачи указываются критерий выгодности маршрута (кратчайший) и соответствующие матрицы расстояний, стоимости и тому подобного. Задача коммивояжёра относится к числу трансвычислительных: уже при относительно небольшом числе городов (66 и более) она не может быть решена методом перебора вариантов никакими теоретически мыслимыми компьютерами за время, меньшее нескольких миллиардов лет.

Различают несколько методов решения такой задачи:

- Решение полным перебором;
- Решение случайным перебором;
- Решение жадными алгоритмами:
    - метод ближайшего соседа;
    - метод включения ближайшего города;
    - метод самого дешёвого включения;
- Решение методом минимального остовного дерева;
- Решение методом имитации отжига;

На практике часто применяются  и различные модификации более эффективных методов: метод ветвей и границ и метод генетических алгоритмов, а также алгоритм муравьиной колонии.

### Блок-схема:

#### Решение задачи коммивояжера методом муравьиной колонии:

![Блок-схема задачи коммивояжера](https://www.bibliofond.ru/wimg/12/607393.files/image046.gif)

### Реализации на разных языках:

#### Python

#### Решение задачи коммивояжёра методом ближайшего соседа:

```sh

import matplotlib.pyplot as plt
import numpy as np
from numpy import exp,sqrt
n=50;m=100;ib=3;way=[];a=0
X=np.random.uniform(a,m,n)
Y=np.random.uniform(a,m,n)
M = np.zeros([n,n]) 
for i in np.arange(0,n,1):
         for j in np.arange(0,n,1):
                  if i!=j:
                           M[i,j]=sqrt((X[i]-X[j])**2+(Y[i]-Y[j])**2)
                  else:
                           M[i,j]=float('inf')           
way.append(ib)
for i in np.arange(1,n,1):
         s=[]
         for j in np.arange(0,n,1):                  
                  s.append(M[way[i-1],j])
         way.append(s.index(min(s)))# Индексы пунктов ближайших городов соседей
         for j in np.arange(0,i,1):
                  M[way[i],way[j]]=float('inf')
                  M[way[i],way[j]]=float('inf')
S=sum([sqrt((X[way[i]]-X[way[i+1]])**2+(Y[way[i]]-Y[way[i+1]])**2) for i in np.arange(0,n-1,1)])+ sqrt((X[way[n-1]]-X[way[0]])**2+(Y[way[n-1]]-Y[way[0]])**2)                      
plt.title('Общий путь-%s.Номер города-%i.Всего городов -%i.\n Координаты X,Y случайные числа от %i до %i'%(round(S,3),ib,n,a,m), size=14)
X1=[X[way[i]] for i in np.arange(0,n,1)]
Y1=[Y[way[i]] for i in np.arange(0,n,1)]    
plt.plot(X1, Y1, color='r', linestyle=' ', marker='o')
plt.plot(X1, Y1, color='b', linewidth=1)   
X2=[X[way[n-1]],X[way[0]]]
Y2=[Y[way[n-1]],Y[way[0]]]
plt.plot(X2, Y2, color='g', linewidth=2,  linestyle='-', label='Путь от  последнего \n к первому городу') 
plt.legend(loc='best')
plt.grid(True)
plt.show()
```

#### С

```sh

 public class FakeMatrix
    {
        private List<Guid> _x = new List<Guid>();
        private List<Guid> _y = new List<Guid>();
        private Dictionary<Tuple<Guid, Guid>, object> _data = new Dictionary<Tuple<Guid, Guid>, object>();
 
        public FakeMatrix(int x, int y)
        {
            for (int i = 0; i < x; i++)
                _x.Add(Guid.NewGuid());
            for (int j = 0; j < y; j++)
                _y.Add(Guid.NewGuid());
            for (int i = 0; i < x; i++)
                for (int j = 0; j < y; j++)
                    _data.Add(Tuple.Create(_x[i], _y[j]), null);
 
        }
        public object this[int x, int y]
        {
            get
            {
                if (x >= 0 && x < _x.Count && y >= 0 && y < _y.Count)
                    return _data[Tuple.Create(_x[x], _y[y])];
                else
                    return null;
            }
            set
            {
                if (x >= 0 && x < _x.Count && y >= 0 && y < _y.Count)
                    _data[Tuple.Create(_x[x], _y[y])] = value;
                else
                    throw new Exception("Хреновые координаты.");
            }
        }
        public void RemoveX(int i)
        {
            if (i >= 0 && i < _x.Count) {
                List<Tuple<Guid, Guid>> remove = _data.Keys.Where(k => k.Item1 == _x[i]).ToList<Tuple<Guid, Guid>>();
                foreach (var item in remove)
                    _data.Remove(item);
                _x.RemoveAt(i);
            }
        }
        public void RemoveY(int i)
        {
            if (i >= 0 && i < _y.Count) {
                List<Tuple<Guid, Guid>> remove = _data.Keys.Where(k => k.Item1 == _y[i]).ToList<Tuple<Guid, Guid>>();
                foreach (var item in remove)
                    _data.Remove(item);
                _y.RemoveAt(i);
            }
        }
 
    }
```

#### C++

```sh

#include <iostream>
using namespace std;
const int inf=1E9,NMAX=16;
int n,i,j,k,m,temp,ans,d[NMAX][NMAX],t[1<<NMAX][NMAX];
bool get(int nmb,int x)
{ return (x&(1<<nmb))!=0; }
int main()
{
 cin >>n;
 for (i=0;i<n;++i)
  for (j=0;j<n;++j) cin>>d[i][j];
 t[1][0]=0; m=1<<n;
 for (i=1;i<m;i+=2)
  for (j=(i==1)?1:0;j<n;++j)
  {
   t[i][j]=inf;
   if (j>0 && get(j,i))
   {
    temp=i^(1<<j);
    for (k=0;k<n;++k) 
     if (get(k,i) && d[k][j]>0) t[i][j]=min(t[i][j],t[temp][k]+d[k][j]);
   }
  }
 for (j=1,ans=inf;j<n;++j)
  if (d[j][0]>0) ans=min(ans,t[m-1][j]+d[j][0]);
 if (ans==inf) cout<<-1; else cout<<ans;
}
```

#### Java

```sh

import java.io.File;
import java.io.IOException;
import java.util.Scanner;
 
public class Graph {
    private int count;
    private int[][] matrix;
    private boolean[] marks;
 
    public Graph(int count){
        this.count=count;
        matrix=new int[count][count];
        marks=new boolean[count];
    }
 
    public void setEdge(int a,int b,int weight){
        matrix[a][b]=weight;
        matrix[b][a]=weight;
    }
 
    public int getEdge(int a,int b){
        return matrix[a][b];
    }
 
    public boolean hasEdge(int a,int b){
        return matrix[a][b]!=0;
    }
 
    public static Graph load(File file) throws IOException {
        Scanner sc=new Scanner(file);
 
        Graph graph=new Graph(sc.nextInt());
 
        while(sc.hasNext()){
            int a=sc.nextInt();
            int b=sc.nextInt();
            int weight=sc.nextInt();
 
            graph.setEdge(a,b,weight);
        }
 
        return graph;
    }
 
    public static Graph load(String filename) throws IOException {
        return load(new File(filename));
    }
 
    public boolean enter(int pos){
        if(marks[pos]){
            return false;
        }else{
            marks[pos]=true;
            return true;
        }
    }
 
    public void leave(int pos){
        marks[pos]=false;
    }
 
    public int getCount(){
        return count;
    }
 
    public boolean allVisited(){
        for(int i=0;i<marks.length;i++){
            if(!marks[i])return false;
        }
        return true;
    }
}
```

#### JavaScript

#### Решаем задачу коммивояжёра простым перебором:

```javascript
var towns = [
	[0, 5, 6, 14, 15],
	[5, 0, 7, 10, 6],
	[6, 7, 0, 8, 7],
	[14, 10, 8, 0, 9],
	[15, 6, 7, 9, 0]
];
var path =[];
var counter = 0;
var minPath = 10000;
var minCounter;
for (var i1 = 0;  i1 <=4; i1++) {
	for (var i2 = 0;  i2 <=4; i2++) {
		for (var i3 = 0;  i3 <=4; i3++) {
			for (var i4 = 0;  i4 <=4; i4++) {
				for (var i5 = 0;  i5 <=4; i5++) {
					if ((i1 != i2) && (i1 != i3) && (i1 != i4) && (i1 != i5) &&(i2 != i3) && (i2 != i4) && (i2 != i5) &&(i3 != i4) && (i3 != i5) &&(i4 != i5)){
                                 path[counter] = (i1+1) + ' → ' + (i2 + 1) +' → ' + (i3 + 1) + ' → ' + (i4 + 1) + ' → ' + (i5 + 1);
                                 console.log(path[counter]);
                                 if ( (towns[i1][i2] + towns[i2][i3] + towns[i3][i4] + towns[i4][i5]) < minPath) {
                                 		minPath = towns[i1][i2] + towns[i2][i3] + towns[i3][i4] + towns[i4][i5];
                                        console.log(minPath);
                                        minCounter = counter;
								}	
								counter +=1;
						}
                    }
				}
			}
		}
};
console.log('Путь с самой короткой длиной маршрута: ' + path[minCounter] + '(' + minPath + ' км.)');
```

### Список источников:

https://habr.com/ru/post/329604/
https://ru.wikipedia.org/wiki/%D0%97%D0%B0%D0%B4%D0%B0%D1%87%D0%B0_%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D0%B2%D0%BE%D1%8F%D0%B6%D1%91%D1%80%D0%B0
https://thecode.media/path-js/
https://studassistent.ru/charp/zadacha-kommivoyazhera-nuzhen-primer-c
