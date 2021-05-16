---
title: Быстрая сортировка
permalink: /article/quicksort/index.html
---

### Область применения:

Этот алгоритм является одним из самых быстрых и универсальных алгоритмов сортировки массивов.
Историческая справка и информация об авторах:
Алгоритм был разработан английским информатиком Тони Хоаром во время его обучения в МГУ в 1960 году, где он обучался компьютерному переводу и занимался разработкой русско-английского разговорника.

### Идея/рисунки/анимация:

Быстрая сортировка является существенно улучшенным вариантом алгоритма сортировки с помощью прямого обмена («Пузырьковая сортировка» и «Шейкерная сортировка»), который известен в том числе своей низкой эффективностью. Принципиальное отличие состоит в том, что в первую очередь производятся перестановки элементов массивов на наибольшем возможном расстоянии и после каждого прохода элементы делятся на две независимые группы.

**Алгоритм быстрой сортировки можно разделить в целом на три шага:**

+ Выбрать элемент из массива. Назовём его опорным.

В ранних реализациях алгоритма опорным выбирался первый элемент, что снижало производительность на отсортированных массивах. Для улучшения эффективности можно выбрать средний элемент, случайный элемент или (для больших массивов) медиана первого, среднего и последнего элементов. Медиана всей последовательности является оптимальным опорным элементом, но её вычисление слишком трудоемко для использования в сортировке. Поэтому лучше выбрать средний элемент в массиве.

+ Разбиение: перераспределение элементов в массиве таким образом, что элементы меньше опорного помещаются перед ним, а больше или равные после.
+ Рекурсивно применить первые два шага к двум подмассивам слева и справа от опорного элемента.

**Рекурсия** - это функция, которая вызывает сама себя. Рекурсия не применяется к массиву, в котором только один элемент или отсутствуют элементы.

### Псевдокод:

```sh
algorithm quicksort(A, low, high) is
    if low < high then
        p:= partition(A, low, high)
        quicksort(A, low, p - 1)
        quicksort(A, p + 1, high)
```

### Блок схема:

![Блок схема алгоритма быстрой сортировки](https://pro-prof.com/wp-content/uploads/2013/03/qsort-293x300.png)

### Реализация на разных языках:

#### С

```sh
int n, a[n];
void qs(int *s_arr, int first, int last) {
    if (first < last) {
        int left = first, right = last, middle = s_arr[(left + right) / 2];
        do {
            while (s_arr[left] < middle) left++;
            while (s_arr[right] > middle) right--;
            if (left <= right) {
                int tmp = s_arr[left];
                s_arr[left] = s_arr[right];
                s_arr[right] = tmp;
                left++;
                right--;
            }
        } while (left <= right);
        qs(s_arr, first, right);
        qs(s_arr, left, last);
    }
}
```
#### С++

```sh
int partition (double *a, int p, int r) {
    double x = *(a+r);
    int i = p - 1;
    int j;
    double tmp;
    for (j = p; j < r; j++) {
      if (*(a+j) <= x) {
        i++;
        tmp = *(a+i);
        *(a+i) = *(a+j);
        *(a+j) = tmp;
      }
    }
    tmp = *(a+r);
    *(a+r) = *(a+i+1);
    *(a+i+1) = tmp;
    return i + 1;
}
 
 void quicksort (double *a, int p, int r) {
    int q;
    if (p < r) {
      q = partition (a, p, r);
      quicksort (a, p, q-1);
      quicksort (a, q+1, r);
    }
 }
 ```

#### Java

```sh
import java.util.Arrays;

public class QuickSort {
	public static void quickSort(int[] array, int low, inthigh) {
    	if (array.length == 0)
        	return;

		if (low >= high)
			return;

		int middle = low + (high - low) / 2;
		int opora = array[middle];

		int i = low, j = high;
		while (i <= j) {
			while (array[i] < opora) {
				i++;
			}

            while (array[j] > opora) {
            	j--;
            }

            if (i <= j) {
            	int temp = array[i];
                array[i] = array[j];
                array[j] = temp;
                i++;
                j--;
            }
		}

		if (low < j)
		quickSort(array, low, j);

		if (high > i)
		quickSort(array, i, high);
		}
        public static void main(String[] args) {
          int[] x = { 8, 0, 4, 7, 3, 7, 10, 12, -3 };
          System.out.println("Было");
          System.out.println(Arrays.toString(x));

          int low = 0;
          int high = x.length - 1;

		quickSort(x, low, high);
			System.out.println("Стало");
		System.out.println(Arrays.toString(x));
	}
 }
```

#### Python

```sh
def qsort(L):
    if L: return qsort(filter(lambda x: x < L[0], L[1:])) + L[0:1] + qsort(filter(lambda x: x >= L[0], L[1:]))
    return []
```

#### JavaScript

```javascript
const qsort = (data, compare, change) => {
    let a = data,
        f_compare = compare,
        f_change  = change;

    if (!(a instanceof Array)) { 
        return undefined;
    };
    if (f_compare === undefined) { 
        f_compare = (a, b) => {
           if (a === b) {
               return 0;
           } else if (a > b) {
               return 1;
           } else {
               return -1;
           }
       }
    };
    if (f_change === undefined) {
        f_change = (a, i, j) => {
        const temp = a[i];
        a[i] = a[j];
        a[j] = temp;
        }
    };

    const qs = (l, r) => {
        let i = l,
            j = r,
            x = a[l+r>>1];

        while (i <= j) {
            while (f_compare(a[i], x) === -1) {
                i++;
            }
            while (f_compare(a[j], x) === 1) {
                j--;
            }
            if (i <= j) {
                f_change(a, i++, j--);
            }
        }
        if (l < j) {
            qs(l, j);
        }
        if (i < r) {
            qs(i, r);
        }
        qs(0, a.length - 1);
    }
}
function QuickSort(A, p, r) {
        if(p<r) {
                var q = Partition(A, p, r);
                QuickSort(A, p, q);
                QuickSort(A, q+1, r);
        }
}
function Partition(A, p, r) {
        var x = A[r];
        var i=p-1;
        for(var j=p; j<=r ;j++ ) {
                if(A[j] <= x) {
                        i++;
                        var temp = A[i];
                        A[i] = A[j];
                        A[j] = temp;
                }
        }
        return i<r ?i : i-1;
}
```

#### Go

```sh
package main
import (
	"fmt"
)
func partition(a []int, lo, hi int) int {
	p := a[hi]
	for j := lo; j < hi; j++ {
		if a[j] < p {
			a[j], a[lo] = a[lo], a[j]
			lo++
		}
	}
	a[lo], a[hi] = a[hi], a[lo]
	return lo
}
func quickSort(a []int, lo, hi int) {
	if lo > hi {
		return
	}
	p := partition(a, lo, hi)
	quickSort(a, lo, p-1)
	quickSort(a, p+1, hi)
}
func main() {
	list := []int{55, 90, 74, 20, 16, 46, 43, 59, 2, 99, 79, 10, 73, 1, 68, 56, 3, 87, 40, 78, 14, 18, 51, 24, 57, 89, 4, 62, 53, 23, 93, 41, 95, 84, 88}
    
	quickSort(list, 0, len(list)-1)
	fmt.Println(list)
```

#### Rust

```sh
#[allow(non_camel_case_types)]
type tupe=i64;
fn quicksort(a:&mut Vec<tupe>)
{
	if a.len()<=1
	{return ();}
	let mut amin=Vec::new();
	let mut amax=Vec::new();
	for _i in &a[1..]
	{
		if *_i>a[0]
		{
			amax.push(*_i);
		}
		else
		{
			amin.push(*_i);
		}
	}
	quicksort(&mut amin);
	quicksort(&mut amax);
	amin.push(a[0]);
	amin.extend(amax);
	for (_num, _i) in amin.iter().enumerate()
	{
		a[_num]=*_i;
	}
}

fn quicksort1(s_arr:&mut [tupe])
{
	let last = s_arr.len()-1;
	if 0 < last
	{
		let mut left = 0;
		let mut right = last;
		let middle = s_arr[ last / 2];
		loop
		{
			while s_arr[left] < middle {left+=1};
			while s_arr[right] > middle {right-=1};
			if left < right
			{
				let tmp = s_arr[left];
				s_arr[left] = s_arr[right];
				s_arr[right] = tmp;
				left+=1;
				right-=1;
			}
			else {break;}
		}
		right-=1;
		left+=1;
		
		quicksort1(&mut s_arr[0..=right]);
		quicksort1(&mut s_arr[left..=last]);
	}
}

fn main()
{
	let listfir = vec![10,29,14,4,35,6];
	let mut list = listfir.clone();
	println!("{:?}", list);
	quicksort(&mut list);
	println!("{:?}", list);
	
	let mut list = listfir.clone();
	println!("{:?}", list);
	quicksort1(&mut list[..]);
	println!("{:?}", list);

}
```

### Список источников:

https://academic.oup.com/comjnl/article/5/1/10/395338
https://ru.wikipedia.org/wiki/%D0%91%D1%8B%D1%81%D1%82%D1%80%D0%B0%D1%8F_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0
https://habr.com/ru/sandbox/29775/
https://cs.stackexchange.com/questions/11458/quicksort-partitioning-hoare-vs-lomuto/11550#11550
https://ru.wikibooks.org/wiki/%D0%A0%D0%B5%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D0%B8_%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC%D0%BE%D0%B2/%D0%A1%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0/%D0%91%D1%8B%D1%81%D1%82%D1%80%D0%B0%D1%8F
https://otus.ru/nest/post/788/
http://citeseer.ist.psu.edu/viewdoc/download;jsessionid=093C91C5D9A30A3F06C6DFA683FAEBDF?doi=10.1.1.14.8162&rep=rep1&type=pdf

