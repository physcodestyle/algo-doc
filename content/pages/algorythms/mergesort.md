---
title: Сортировка слиянием
permalink: /algorythms/mergesort/index.html
eleventyNavigation:
    key: mergesort
    order: 2
    title: Сортировка слиянием
---

### Область применения:

Сортировка слиянием применима для сортировки массивов связанных списков за O(nLogn) время. Алгоритм сортировки слиянием относится к стратегии “разделяй и властвуй” (также как бинарный поиск и быстрая сортировка). Алгоритмы на базе этого принципа являются рекурсивными. Основной принцип работы данного подхода - это разбиение задачи на несколько подзадач меньшего размера. После решения таких подзадач они обратно объединяются. В случае сортировки массива принято рекурсивно разбивать его до единичных или пустых массивов. Единичный массив уже является упорядоченным.

### Историческая справка и информация об авторах:

История алгоритма начинается в двадцатом веке, когда стали появляться первые сортировочные устройства и в дальнейшем вычислительные машины. Разработка методов сортировки тесно переплеталась с развитием ЭВМ. Имеются свидетельства того, что программа сортировки была первой когда-либо написанной для вычислительных машин с запоминаемой программой. Одним из основоположников вычислительной техники был Фон Нейман. В 1945 году он изобрёл алгоритм сортировки слиянием. Фон Нейман написал чернилами 23-страничную программу сортировки для EDVAC (одна из первых ЭВМ). 

### Идея: 

Чтобы рассортировать массив с числами методом слияния нужно разделить его на подмассивы, состоящие из двух чисел либо, если массив имеет нечетную длину, одного числа. Далее идет процесс упорядочивания этих подмассивов путем сравнения их элементов. Следующий шаг - это обратное объединение уже упорядоченных подмассивов с параллельной их сортировкой.

Рассмотрим пример. Есть массив с данными, и его нужно отсортировать.

[14, 8, 2, 19, 5, 1, 10]

Шаг 1: Мы должны разбить исходный массив на подмассивы, получим:

[14, 8] [2, 19] [5, 1] [10]

Шаг 2: Сортируем значения в новых массивах, получим:

[8, 14] [2, 19] [1, 5] [10]

Шаг 3: Начинаем объединять массивы, сравнивая значения одного массива с другим:

8 > 2 => [2, 8]
8 < 19 => [2, 8, 19]
14 < 19 => [2, 8, 14, 19]

1 < 10 => [1, 10]
5 < 10 => [1, 5, 10]

Получили: 

[2, 8, 14, 19] [1, 5, 10]

Повторяем шаг 3 и получим упорядоченный массив: 

[1, 2, 5, 8, 10, 14, 19]

![mergesort](/content/images/mergesort.png)


*Преимущества:*
+ Позволяет хранить данные и программу в памяти компьютера в одном и том же адресном пространстве.
+ Хорошо применим к большим наборам данных.

*Недостатки:* 
+ Хорошо подходит для больших задач, но является более медленным по сравнению с другими алгоритмами сортировки для небольших задач.
+ Алгоритм сортировки слиянием требует дополнительного пространства памяти 0(n) для временного массива.
+ Он проходит через весь набор данных, даже если массив отсортирован.

### Псевдокод:

```sh
  L = *In1;
  R = *In2;
  if( L == R )
  {
    *Out++ = L;
    In1++;
    *Out++ = R;
    In2++;
  }
  else if( L < R )
  {
    *Out++ = L;
    In1++;
  }
  else
  {
    *Out++ = R;
    In2++;
  }
```

### Реализации на разных языках:

#### C
```sh
void mergesort(long num, float a[])
{
  int rght, rend;
  int i, j, m;

  for (int k = 1; k < num; k *= 2) {
    for (int left = 0; left + k < num; left += k * 2) {
      rght = left + k;
      rend = rght + k;
      if (rend > num) rend = num;
      m = left; i = left; j = rght;
      while (i < rght && j < rend) {
        if (a[i] <= a[j]) {
          b[m] = a[i]; i++;
        } else {
          b[m] = a[j]; j++;
        }
        m++;
      }
      while (i < rght) {
        b[m] = a[i];
        i++; m++;
      }
      while (j < rend) {
        b[m] = a[j];
        j++; m++;
      }
      for (m = left; m < rend; m++) {
        a[m] = b[m];
      }
    }
  }
}
```

#### C++

```sh
#include <iostream>
#include <conio.h>
using namespace std;
#define maxn 100

int a[maxn];
int n;

void merge(int l, int r) {
  if (r == l)
  return;
  if (r — l == 1) {
    if (a[r] < a[l]) swap(a[r], a[l]);
    return;
  }
  int m = (r + l) / 2;
  merge(l, m);
  merge(m + 1, r);
  int buf[maxn];
  int xl = l;
  int xr = m + 1;
  int cur = 0;
  while (r — l + 1 != cur) {
    if (xl > m)
      buf[cur++] = a[xr++];
    else if (xr > r)
      buf[cur++] = a[xl++];
    else if (a[xl] > a[xr])
      buf[cur++] = a[xr++];
    else buf[cur++] = a[xl++];
  }
  for (int i = 0; i < cur; i++)
    a[i + l] = buf[i];
}
int main() {
  cin >> n;
  for (int i = 0; i < n; i++)
  cin >> a[i];
  merge(0, n — 1);
  for (int i = 0; i < n; i++)
  cout << a[i] << » «;
  getch();
  return 0;
}
```

#### Java

```sh
public int [] sortArray(int[] arrayA){ 
  if (arrayA == null) {
    return null;
  }
  if (arrayA.length < 2) {
    return arrayA; 
  }
  int [] arrayB = new int[arrayA.length / 2];
  System.arraycopy(arrayA, 0, arrayB, 0, arrayA.length / 2);

  int [] arrayC = new int[arrayA.length - arrayA.length / 2];
  System.arraycopy(arrayA, arrayA.length / 2, arrayC, 0, arrayA.length - arrayA.length / 2);

  arrayB = sortArray(arrayB); 
  arrayC = sortArray(arrayC); 

  return mergeArray(arrayB, arrayC);
}
```

#### Python

```sh
def merge_sort(alist, start, end):
  if end - start > 1:
    mid = (start + end)//2
    merge_sort(alist, start, mid)
    merge_sort(alist, mid, end)
    merge_list(alist, start, mid, end)
 
def merge_list(alist, start, mid, end):
  left = alist[start:mid]
  right = alist[mid:end]
  k = start
  i = 0
  j = 0
  while (start + i < mid and mid + j < end):
    if (left[i] <= right[j]):
      alist[k] = left[i]
      i = i + 1
    else:
      alist[k] = right[j]
      j = j + 1
    k = k + 1
  if start + i < mid:
    while k < end:
      alist[k] = left[i]
      i = i + 1
      k = k + 1
    else:
      while k < end:
        alist[k] = right[j]
        j = j + 1
        k = k + 1
 
alist = input('Enter the list of numbers: ').split()
alist = [int(x) for x in alist]
merge_sort(alist, 0, len(alist))
print('Sorted list: ', end='')
print(alist)
```

#### JavaScript

```sh
function mergeSort(array)
{
	if (array.length > 1)	{
		let mid = Math.floor(array.length/2);
    let lefthalf = array.slice(0,mid);
    let righthalf = array.slice(mid);
		mergeSort(lefthalf);
		mergeSort(righthalf);

		let i = j = k = 0;
		while (i < lefthalf.length && j < righthalf.length)	{
			if (lefthalf[i] < righthalf[j])	{
				array[k] = lefthalf[i];
				i++;
			}	else {
				array[k] = righthalf[j];
				j++;
			}
			k++;
		}
		while (i < lefthalf.length) {
			array[k] = lefthalf[i];
			i++;
			k++;
		}
		while(j < righthalf.length)	{
			array[k] = righthalf[j];
			j++;
			k++;
		}
	}
	return array
}
```

#### Go

```sh
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	slice := generateSlice(20)
	fmt.Println("\n--- Unsorted --- \n\n", slice)
	fmt.Println("\n--- Sorted ---\n\n", mergeSort(slice), "\n")
}

func generateSlice(size int) []int {
	slice := make([]int, size, size)
	rand.Seed(time.Now().UnixNano())
	for i := 0; i < size; i++ {
		slice[i] = rand.Intn(999) - rand.Intn(999)
	}
	return slice
}
 
func mergeSort(items []int) []int {
    var num = len(items)     
    if num == 1 {
        return items
    }     
    middle := int(num / 2)
    var (
        left = make([]int, middle)
        right = make([]int, num-middle)
    )
    for i := 0; i < num; i++ {
        if i < middle {
            left[i] = items[i]
        } else {
            right[i-middle] = items[i]
        }
    }     
    return merge(mergeSort(left), mergeSort(right))
}
 
func merge(left, right []int) (result []int) {
    result = make([]int, len(left) + len(right))
    i := 0
    for len(left) > 0 && len(right) > 0 {
        if left[0] < right[0] {
            result[i] = left[0]
            left = left[1:]
        } else {
            result[i] = right[0]
            right = right[1:]
        }
        i++
    }
    for j := 0; j < len(left); j++ {
        result[i] = left[j]
        i++
    }
    for j := 0; j < len(right); j++ {
        result[i] = right[j]
        i++
    }
    return
}
```

#### Rust

```sh
pub fn insert_sort(a: &mut Vec<f64>) {
  let mut swap = true;
  while swap {
    swap = false;
    for i in 0..a.len()-1 {
      if a[i] > a[i+1] {
          swap = true;
          a.swap(i , i+1);
      }
    }
  }
}
pub fn merge_sort(a: &mut Vec<f64>, b: usize, e: usize) {
  if b < e {
    let m = (b+e)/2;
    merge_sort(a, b, m);
    merge_sort(a, m+1, e);
    merge(a, b, m, e);
  }
}
fn merge(a: &mut Vec<f64>, b: usize, m:usize, e:usize) {
  let mut left = a[b..m+1].to_vec();
  let mut right = a[m+1..e+1].to_vec();
  left.reverse();
  right.reverse();
  println!("size of right: {}", right.len());
  for k in b..e + 1 {
    if left.is_empty() {
      a[k] = right.pop().unwrap();
      continue;
    }
    if right.is_empty() {
      a[k] = left.pop().unwrap();
      continue;
    }
    if right.last() < left.last() {
      a[k] = right.pop().unwrap();
    }
    else {
      a[k] = left.pop().unwrap();
    }
  }
}
```

### Список источников: 

+ Дональд Кнут: Искусство программирования. Том 3. Сортировка и поиск
+ Papers of John Von Neumann on Computing and Computer Theory
+ Адитья Бхаргава "Грокаем алгоритмы"
+ [Merge sort](https://www.geeksforgeeks.org/merge-sort/)


