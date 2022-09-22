---
title: Сортировка выбором
date: Last Modified
permalink: /article/Selection/index.html
eleventyNavigation:
key: Selection.Sort
order: 2
title: Сортировка выбором
---
### 1.Область применения
Сортировка влияет на скорость алгоритма, в котором нужно обратиться к определённому элементу массива. Когда элементы отсортированы, их проще найти, производить с ними различные операции.
Сортировка выбором — это алгоритм сортировки массивов, в котором на каждой итерации во всей последовательности неотсортированных данных выбирается минимальный элемент (при сортировке по возрастанию) и помещается в первую позицию неотсортированной последовательности. Другой вариант данного алгоритма -  нахождение  максимального элемента и помещение его в конец неотсортированной последовательности.  
В данной статье мы рассмотрим сортировку с нахождением минимального элемента массива.


### 2.Идея(рисунки/анимации/видео)
Суть алгоритма состоит в том, чтобы находить в массиве наименьший элемент и переносить его в начало массива, элементы корого пока не отсортированы.   
**Алгоритм:**     
1. Пусть первый элемент в массиве наименьший
2. Сравниваем этот элемент с другими элементами в массиве
3. Находим минимальный элемент и меняем его местами с первым элементом в неотсортированном массиве (обмен не нужен, если наименьший элемент уже в начале массива)
4. Повторяем пункты  1-3 для неотсортированной части  

**Пример**  
Пусть есть массив [1,3,5,2,6]  
Пусть первый элемент (1) - минимальный. Сравниваем его с другими элементами.    
Так как 1 в массиве - минимальное число, мы ничего не меняем. Теперь работаем с неотсортированной частью массива.  
Сравниваем второй элемент массива (3) с другими неотсортированными элементами. 2 меньше 3, поэтому теперь 2 - минимальное значение. 
Меняем местами 2 и 3. Теперь наш массив имеет вид [1,2,5,3,6].  
Сравниваем третий элемент массива (5) с другими неотсортированными элементами. 3 меньше 5, поэтому теперь 3 - минимальное значение.  
Меняем местами 3 и 5. теперь наш массив имеет вид [1,2,3,5,6].   
Сравниваем четвертый элемент массива (5) с последним элементом. 5 меньше 6, поэтому мы ничего не меняем.  
Отсортированный массив имеет вид [1,2,3,5,6].

*В данных gif-файлах красным помечен элемент, который мы запоминаем как минимальный, а желтым - элемент в отсортированном массиве*
![Alt Text](https://media.proglib.io/wp-uploads/-000//1/596b722fd2a7d_djIHfUK.gif)
![Alt Text](https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif)

[Видео-реализация алгоритмов сортировки](https://airtucha.github.io/SortVis/)  


### 3.Псевдокод/блок-схема
Пусть есть массив A с количеством элементов n
```sh

 выполнять n-1 раз 
 запоминть первый элемент как минимальный
 для каждого неотсортированного элемента:
   Если элемент меньше нашего минимального элемента
     сделать этот эелемент минимальным
   поменять минимальный элемент с первым элементом в неотсортированной части массива
```
```sh
SelectionSort(A):
  for j ← 1 to n-1
    smallest ← j
    for i ← j + 1 to n
      if A[i] < A[smallest]
        smallest ← i
    Swap A[j] ↔ A[smallest]
```

### 4.Реализации на разных языках
### C
```sh
for (int i = 0; i < size - 1; i++) 
{
    int min_i = i;
	for (int j = i + 1; j < size; j++) 
    {
		if (array[j] < array[min_i]) 
        {
			min_i = j;
		}
	}
	int temp = array[i];
	array[i] = array[min_i];
	array[min_i] = temp;
}
```
### C++
```sh
template <typename T, typename C = less< typename T::value_type> >
void select_sort( T f, T l, C c = C() )
{
    if (f!= l) {
        while (f != l - 1) {
            T min = f;
            for (T i = f + 1; i != l; i++) {
                if (c(*i, *min)) {
                    typename T::value_type tmp = *min;
                    *min = *i;
                    *i = tmp;
                }
            }
            f++;
        }
    }
}
```
### Java
##### Без рекурсии
```sh
public static void selectionSort (int[] numbers){
    int min, temp;

    for (int index = 0; index < numbers.length-1; index++){
        min = index;
        for (int scan = index+1; scan < numbers.length; scan++){
            if (numbers[scan] < numbers[min])
                min = scan;
        }

        // Swap the values
        temp = numbers[min];
        numbers[min] = numbers[index];
        numbers[index] = temp;
    }
}
```

### Python
##### [Устойчивая](https://ru.wikipedia.org/wiki/Устойчивая_сортировка "не меняет относительный порядок сортируемых элементов, имеющих одинаковые ключи, по которым происходит сортировка")
```sh
def select_sort_stable(arr):
    if len(arr) == 0: return
    for j in range(len(arr)):
        min = j
        for i in range(j + 1, len(arr)):
            if arr[i] < arr[min]: min = i
        if min != j:
            value = arr[min]
            for l in range(min, j - 1, -1):
                arr[l] = arr[l - 1]
            arr[j] = value
```
##### [Неустойчивая](https://ru.wikipedia.org/wiki/Устойчивая_сортировка "меняет относительный порядок сортируемых элементов, имеющих одинаковые ключи, по которым происходит сортировка")
```sh
def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]

def select_sort(arr):
    i = len(arr)
    while i > 1:
        max = 0
        for j in range(i):
            if arr[j] > arr[max]:
                max = j
        swap(arr, i - 1, max)
        i -= 1
```
### Pascal
##### Без рекурсии
```sh
for i := 1 to n - 1 do begin
    min := i;
    for j := i + 1 to n do
        if a[min] > a[j] then
            min := j;
    if min<>i then begin
        t := a[i];
        a[i] := a[min];
        a[min] := t;
    end;
end;
```
##### С рекурсией
```sh
public static int findMin(int[] array, int index){
    int min = index - 1;
    if(index < array.length - 1) min = findMin(array, index + 1);
    if(array[index] < array[min]) min = index;
    return min;
}

public static void selectionSort(int[] array){
    selectionSort(array, 0);
}

public static void selectionSort(int[] array, int left){
    if (left < array.length - 1) {
        swap(array, left, findMin(array, left));
        selectionSort(array, left+1);
    }
}

public static void swap(int[] array, int index1, int index2) {
    int temp = array[index1];
    array[index1] = array[index2];
    array[index2] = temp;
}

```
### PHP
```sh
$size = count($arr);
for ($i = 0; $i < $size-1; $i++)
{
    $min = $i;
    for ($j = $i + 1; $j < $size; $j++)
    {
        if ($arr[$j] < $arr[$min])
        {
            $min = $j;
        }
    }
    $temp = $arr[$i];
    $arr[$i] = $arr[$min];
    $arr[$min] = $temp;
}
```

### 5.Список источников
https://www.geeksforgeeks.org/selection-sort/  
https://ru.wikibooks.org/wiki/Реализации_алгоритмов   
https://iq.opengenus.org/selection-sort/
