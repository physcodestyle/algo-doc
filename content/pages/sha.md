---
title: SHA - алгоритм хеширования
permalink: algorythms/sha/index.html
---

### Область применения:

Secure Hash Algorithm (SHA) - алгоритм безопасного хеширования, он предназначен для криптографической безопасности.

Алгоритмом хеширования называют математическую функцию, которая уплотняет данные до фиксированного размера. Например берем предложение "Что такое хеширование?" пропускаем его через специальный алгоритм хеширования и получаем сообщение: "436cef0eb76766bd4202c7f80a8e775e356d21dc" - этот результат называется хешем от приведенного предложения. Хеширование используется для разных целей, можно использовать для сравнения двух файлов, ведь проще сначала вычислить их хеши и сравнить их хеши, чем посимвольно сравнивать два файла. Очень удобно и безопасно получается например для хранения паролей в базе - достаточно сравнить хеш установленного пароля и хеш введенного пароля, чтобы удостовериться, что введен правильный пароль. А дополнительным плюсом будет то, что пароль не хранится в базе, с точки зрения безопасности это является преимуществом.

### Идея/рисунки/анимация:
Существует множество алгоритмов хеширования, и все они имеют конкретные цели - некоторые оптимизированы для определенных типов данных, другие - для скорости, безопасности и т. д.
Мы поговорим про SHA-1, SHA-2, SHA-3.

Secure Hash Algorithm 1 (SHA-1) - алгоритм криптографического хеширования. Для входного сообщения произвольной длины (максимум 264- 1 бит, что примерно равно 2 эксабайта) алгоритм генерирует 160-битное (20 байт) хеш-значение, которое обычно отображается как шестнадцатеричное число длиной в 40 цифр. Используется во многих криптографических приложениях и протоколах. SHA-1 является наиболее распространенным из всего семейства SHA и применяется в различных широко распространенных криптографических приложениях и алгоритмах. 

Используется в следующих приложениях:
* PGP - для создания электронной цифровой подписи;
* Git - для идентификации каждого объекта по SHA-1-хешу от хранимой в объекте информации;
* Mercurial - для идентификации ревизий;
* BitTorrent - для проверки целостности загружаемых данных.

Алгоритм SHA-1 построен на идее функции сжатия. Входами функции сжатия являются блок сообщения длиной 512 бит и выход предыдущего блока сообщения. Выход представляет собой значение всех хеш-блоков до этого момента. Таким образом, хеш-блок Mi равен hi = f(Mi, hi-1). Хеш-значением всего сообщения является выход последнего блока. Поступает сообщение, оно разбивается на блоки по 512 бит в каждом. Последний блок дополняется до длины, кратной 512 бит. Сначала добавляется 1 (бит), а потом - нули, чтобы длина блока стала равной 512 - 64 = 448 бит. В оставшиеся 64 бита записывается длина исходного сообщения в битах (в big-endian формате).

***big-endian*** - порядок от старшего к младшему: An-1, ... , A0. Этот порядок подобен привычному порядку записи (например арабскими цифрами) «слева-направо», например, число сто двадцать три было бы записано при таком порядке как 123. В этом же порядке принято записывать байты в технической и учебной литературе, если другой порядок явно не обозначен.

Возвращаемся к алгоритму. Если последний блок имеет длину более 447, но менее 512 бит, то дополнение выполняется следующим образом: сначала добавляется 1 (бит), затем - нули вплоть до конца 512-битного блока. После этого создается ещё один 512-битный блок, который заполняется вплоть до 448 бит нулями, после чего в оставшиеся 64 бита записывается длина исходного сообщения в битах (в big-endian формате). Дополнение последнего блока осуществляется всегда, даже если сообщение уже имеет нужную длину. Инициализируются пять 32-битовых переменных:

A = 0x67452301;
B = 0xEFCDAB89;
C = 0x98BADCFE;
D = 0x10325476;
E = 0xC3D2E1F0.

Определяются четыре нелинейные операции и четыре константы:


| Ft(m, l, k) = (m ∧ l) ∨ (¬m ∧ l)| Kt = 0x5A827999 | 0 ≤ t ≤ 19 |
| :-----------------------------|:----------------| :------------|
| Ft(m, l, k) = m ⊕ l ⊕ k       | Kt = 0x6ED9EBA1 | 20 ≤ t ≤ 39  |
| Ft(m, l, k) =  (m ∧ l) ∨ (m ∧ l) ∨ (l ∧ k)| Kt = 0x8F1BBCDC | 40 ≤ t ≤ 59 |
| Ft(m, l, k) = m ⊕ l ⊕ k       | Kt = 0xCA62C1D6 | 60≤t≤79      |
	

Главный цикл итеративно обрабатывает каждый 512-битный блок. В начале каждого цикла вводятся переменные a, b, c, d, e, которые инициализируются значениями A, B, C, D, E, соответственно. Блок сообщения преобразуется из 16 32-битовых слов Mi в 80 32-битовых слов Wj по следующему правилу:

Wt= Mt, при 0 ≤ t ≤ 15
wt = (Wt-3 ⊕ Wt-8 ⊕ Wt-14 ⊕ Wt-16) ≪ 1 , при 16 ≤ t ≤ 79 , где "<<" - это циклический сдвиг влево.

Таким образом, для t от 0 до 79 будет выполнено следующее:

temp = (a ≪ 5) + Ft(b, c, d) + e + Wt + Kt ;
e = d;
d = c;
c = b ≪ 30;
b = a;
a = temp , где "+" - сложение беззнаковых 32-битных целых чисел с отбрасыванием избытка (33-го бита).

После этого к A, B, C, D, E прибавляются значения a, b, c, d, e, соответственно. Начинается следующая итерация.
Итоговым значением будет объединение пяти 32-битовых слов (A, B, C, D, E) в одно 160-битное хеш-значение.

### Псевдокод алгоритма SHA-1:

Замечание: Все используемые переменные 32 бита.

Инициализация переменных:
h0 = 0x67452301
h1 = 0xEFCDAB89
h2 = 0x98BADCFE
h3 = 0x10325476
h4 = 0xC3D2E1F0

```sh 
Предварительная обработка:
Присоединяем бит '1' к сообщению;
Присоединяем k битов '0', где k наименьшее число ≥ 0 такое, что длина получившегося сообщения (в битах) сравнима по модулю 512 с 448 (length mod 512 == 448);
Добавляем длину исходного сообщения (до предварительной обработки) как целое 64-битное
Big-endian число, в битах.

В процессе сообщение разбивается последовательно по 512 бит:
for перебираем все такие части
разбиваем этот кусок на 16 частей, слов по 32-бита (big-endian) w[i], 0 <= i <= 15

    16 слов по 32-бита дополняются до 80 32-битовых слов:
    for i from 16 to 79
        w[i] = (w[i-3] xor w[i-8] xor w[i-14] xor w[i-16]) циклический сдвиг влево 1

    Инициализация хеш-значений этой части:
    a = h0
    b = h1
    c = h2
    d = h3
    e = h4

    Основной цикл:
    for i from 0 to 79
        if 0 ≤ i ≤ 19 then
            f = (b and c) or ((not b) and d)
            k = 0x5A827999
        else if 20 ≤ i ≤ 39 then
            f = b xor c xor d
            k = 0x6ED9EBA1
        else if 40 ≤ i ≤ 59 then
            f = (b and c) or (b and d) or (c and d)
            k = 0x8F1BBCDC
        else if 60 ≤ i ≤ 79 then
            f = b xor c xor d
            k = 0xCA62C1D6

        temp = (a leftrotate 5) + f + e + k + w[i]
        e = d
        d = c
        c = b leftrotate 30
        b = a
        a = temp

    Добавляем хеш-значение этой части к результату:
    h0 = h0 + a
    h1 = h1 + b 
    h2 = h2 + c
    h3 = h3 + d
    h4 = h4 + e

Итоговое хеш-значение(h0, h1, h2, h3, h4 должны быть преобразованы к big-endian):
digest = hash = h0 append h1 append h2 append h3 append h4
```

### Идея/рисунки/анимация:
SHA-1 и SHA-2 отличаются в получении хеша, он создается из исходных данных и по длине в битах подписи. SHA-1 - это 160-битный хеш. SHA-2 является «семейством» хэшей и имеет различную длину, наиболее популярной из которых является 256-битная. Есть «SHA-224», «SHA-384» «SHA-512» и т.д. С 2011 по 2015 год SHA-1 был основным алгоритмом, с 2016 года SHA-2 являлась новым стандартом. Хеш-функции семейства SHA-2 построены на основе структуры Меркла - Дамгарда.
Сообщение разбивается на блоки, каждый блок - на 16 слов. Алгоритм пропускает каждый блок сообщения через цикл с 64 итерациями. На каждой итерации 2 слова преобразуются, функцию преобразования задают остальные слова. Результаты обработки каждого блока складываются, сумма является значением хеш-функции. Так как инициализация внутреннего состояния производится результатом обработки предыдущего блока, то нет возможности обрабатывать блоки параллельно.

**Алгоритм используется:**

* Bitcoin - эмиссия криптовалюты через поиск отпечатков с определёнными рамками значений;
* DNSSEC - дайджесты DNSKEY;
* DSA - используется для создания электронной цифровой подписи;
* IPSec - в протоколах ESP и IKE;
* OpenLDAP - хеши паролей;
* PGP - используются для создания электронной цифровой подписи;
* S/MIME - дайджесты сообщений;
* SHACAL-2 - блочный алгоритм шифрования;
* X.509 - используются для создания электронной цифровой подписи сертификата.

Ввиду алгоритмической схожести SHA-2 с SHA-1 и наличия у последней потенциальных уязвимостей принято решение, что SHA-3 будет базироваться на совершенно ином алгоритме. 2 октября 2012 года NIST утвердил в качестве SHA-3 алгоритм Keccak.

### Псевдокод алгоритма SHA-2 для расчёта отпечатков:
```sh 
Пояснения:
Все переменные беззнаковые, имеют размер 32 бита и при вычислениях суммируются по модулю 232
message - исходное двоичное сообщение
m - преобразованное сообщение
Инициализация переменных
(первые 32 бита дробных частей квадратных корней первых восьми простых чисел [от 2 до 19]):
h0 := 0x6A09E667
h1 := 0xBB67AE85
h2 := 0x3C6EF372
h3 := 0xA54FF53A
h4 := 0x510E527F
h5 := 0x9B05688C
h6 := 0x1F83D9AB
h7 := 0x5BE0CD19
Таблица констант
(первые 32 бита дробных частей кубических корней первых 64 простых чисел [от 2 до 311]):
k[0..63] :=
0x428A2F98, 0x71374491, 0xB5C0FBCF, 0xE9B5DBA5, 0x3956C25B, 0x59F111F1, 0x923F82A4, 0xAB1C5ED5,
0xD807AA98, 0x12835B01, 0x243185BE, 0x550C7DC3, 0x72BE5D74, 0x80DEB1FE, 0x9BDC06A7, 0xC19BF174,
0xE49B69C1, 0xEFBE4786, 0x0FC19DC6, 0x240CA1CC, 0x2DE92C6F, 0x4A7484AA, 0x5CB0A9DC, 0x76F988DA,
0x983E5152, 0xA831C66D, 0xB00327C8, 0xBF597FC7, 0xC6E00BF3, 0xD5A79147, 0x06CA6351, 0x14292967,
0x27B70A85, 0x2E1B2138, 0x4D2C6DFC, 0x53380D13, 0x650A7354, 0x766A0ABB, 0x81C2C92E, 0x92722C85,
0xA2BFE8A1, 0xA81A664B, 0xC24B8B70, 0xC76C51A3, 0xD192E819, 0xD6990624, 0xF40E3585, 0x106AA070,
0x19A4C116, 0x1E376C08, 0x2748774C, 0x34B0BCB5, 0x391C0CB3, 0x4ED8AA4A, 0x5B9CCA4F, 0x682E6FF3,
0x748F82EE, 0x78A5636F, 0x84C87814, 0x8CC70208, 0x90BEFFFA, 0xA4506CEB, 0xBEF9A3F7, 0xC67178F2
Предварительная обработка:
m:= message ǁ [единичный бит]
m:= m ǁ [k нулевых бит], где k - наименьшее неотрицательное число, такое что (L + 1 + K) mod 512 = 448, где L - число бит в сообщении (сравнима по модулю 512 c 448).
m:= m ǁ Длина(message) - длина исходного сообщения в битах в виде 64-битного числа с порядком байтов от старшего к младшему.
Далее сообщение обрабатывается последовательными порциями по 512 бит:
разбить сообщение на куски по 512 бит;
для каждого куска разбить кусок на 16 слов длиной 32 бита (с порядком байтов от старшего к младшему внутри слова): w[0..15].
Сгенерировать дополнительные 48 слов:
для i от 16 до 63
s0 := (w[i-15] rotr 7) xor (w[i-15] rotr 18) xor (w[i-15] shr 3)
s1 := (w[i-2] rotr 17) xor (w[i-2] rotr 19) xor (w[i-2] shr 10)
w[i] := w[i-16] + s0 + w[i-7] + s1
Инициализация вспомогательных переменных:
a := h0
b := h1
c := h2
d := h3
e := h4
f := h5
g := h6
h := h7
Основной цикл:
для i от 0 до 63
Σ0 := (a rotr 2) xor (a rotr 13) xor (a rotr 22)
Ma := (a and b) xor (a and c) xor (b and c)
t2 := Σ0 + Ma
Σ1 := (e rotr 6) xor (e rotr 11) xor (e rotr 25)
Ch := (e and f) xor ((not e) and g)
t1 := h + Σ1 + Ch + k[i] + w[i]
h := g
g := f
f := e
e := d + t1
d := c
c := b
b := a
a := t1 + t2
Добавить полученные значения к ранее вычисленному результату:
h0 := h0 + a
h1 := h1 + b
h2 := h2 + c
h3 := h3 + d
h4 := h4 + e
h5 := h5 + f
h6 := h6 + g
h7 := h7 + h
Получить итоговое значения хеша:
digest = hash = h0 ǁ h1 ǁ h2 ǁ h3 ǁ h4 ǁ h5 ǁ h6 ǁ h7
```
### Идея/рисунки/анимация:

SHA-3 (Keccak) - алгоритм хеширования переменной разрядности, разработанный группой авторов во главе с Йоаном Дайменом.  2 октября 2012 года Keccak стал победителем конкурса криптографических алгоритмов, проводимого Национальным институтом стандартов и технологий США.
Алгоритм SHA-3 построен по принципу криптографической «губки». При котором данные сначала «впитываются» в губку: исходное сообщение M, подвергается многораундовым перестановкам f, затем результат Z «отжимается» из губки. На этапе «впитывания» блоки сообщения суммируются по модулю 2 с подмножеством состояния, после чего всё состояние преобразуется с помощью функции перестановки f. На этапе «отжимания» выходные блоки считываются из одного и того же подмножества состояния, изменённого функцией перестановок f. Размер части состояния, который записывается и считывается, называется «скоростью» и обозначается r, а размер части, которая не тронута вводом/выводом, называется «ёмкостью» и обозначается c.

**Алгоритм получения значения хеш-функции можно разделить на несколько этапов:**
* Исходное сообщение M дополняется до строки длины Р, кратной r, с помощью функции дополнения (pad-функции);
* Строка P делится на n блоков длины r: P0 ,P1, ... , Pn-1;
* «Впитывание»: каждый блок Pi дополняется нулями до строки длины b бит и суммируется по модулю 2 со строкой состояния S, где S - строка длины b бит (b = r + c). Перед началом работы функции все элементы S равны нулю. Для каждого следующего блока состояние - строка, полученная применением функции перестановок f к результату предыдущего шага;
* «Отжимание»: пока длина Z меньше d (d - количество бит в результате хеш-функции), к Z добавляется r первых бит состояния S, после каждого прибавления к S применяется функция перестановок f. Затем Z обрезается до длины d бит;
* Строка Z длины d бит возвращается в качестве результата.

Благодаря тому, что состояние содержит c дополнительных бит, алгоритм устойчив к атаке удлинением сообщения, к которой восприимчивы алгоритмы SHA-1 и SHA-2.
В SHA-3 состояние S - это массив 5 × 5 слов длиной w = 64 бита, всего 5 × 5 × 64 = 1600 бит. Также в Keccak могут использоваться длины w, равные меньшим степеням 2 (от w = 1 до w = 32).
### Реализация на разных языках:
***SHA-3***

**С#**
```sh
static void Main(string[] args)
{
int[] HashArray = new int[4] { 224, 256, 384, 512 };
string Data = "This is the SHA-3 Demonstration";

            Console.WriteLine("Demonstration of SHA-3\n");
 
            for (int Index = 0; Index < HashArray.Length; Index++)
            {
                using (var SHA3Provider = new SHA3.SHA3Managed(HashArray[Index]))
                {
                    Console.WriteLine("Hash Length : {0}", HashArray[Index]);
                    Console.WriteLine("Hash : {0}\n", BitConverter.ToString(SHA3Provider.ComputeHash(Encoding.Default.GetBytes(Data))));
                }
            }
 
            Console.WriteLine("End");
            Console.ReadLine();
        }
```
***SHA-256***

**C#**
```sh
using System;
using System.IO;
using System.Security.Cryptography;

public class HashDirectory
{
public static void Main(String[] args)
{
if (args.Length < 1)
{
Console.WriteLine("No directory selected.");
return;
}

        string directory = args[0];
        if (Directory.Exists(directory))
        {
            // Create a DirectoryInfo object representing the specified directory.
           var dir = new DirectoryInfo(directory);
            // Get the FileInfo objects for every file in the directory.
            FileInfo[] files = dir.GetFiles();
            // Initialize a SHA256 hash object.
            using (SHA256 mySHA256 = SHA256.Create())
            {
                // Compute and print the hash values for each file in directory.
                foreach (FileInfo fInfo in files)
                {
                    try {
                        // Create a fileStream for the file.
                        FileStream fileStream = fInfo.Open(FileMode.Open);
                        // Be sure it's positioned to the beginning of the stream.
                        fileStream.Position = 0;
                        // Compute the hash of the fileStream.
                        byte[] hashValue = mySHA256.ComputeHash(fileStream);
                        // Write the name and hash value of the file to the console.
                        Console.Write($"{fInfo.Name}: ");
                        PrintByteArray(hashValue);
                        // Close the file.
                        fileStream.Close();
                    }
                    catch (IOException e) {
                        Console.WriteLine($"I/O Exception: {e.Message}");
                    }
                    catch (UnauthorizedAccessException e) {
                        Console.WriteLine($"Access Exception: {e.Message}");
                    }
                }
            }
        }
        else
        {
            Console.WriteLine("The directory specified could not be found.");
        }
    }
 
    // Display the byte array in a readable format.
    public static void PrintByteArray(byte[] array)
    {
        for (int i = 0; i < array.Length; i++)
        {
            Console.Write($"{array[i]:X2}");
            if ((i % 4) == 3) Console.Write(" ");
        }
        Console.WriteLine();
    }
}
```
***SHA-1***

**C#**
```sh
static string Hash(string input)
{
using (SHA1Managed sha1 = new SHA1Managed())
{
var hash = sha1.ComputeHash(Encoding.UTF8.GetBytes(input));
var sb = new StringBuilder(hash.Length * 2);

            foreach (byte b in hash)
            {
                // can be "x2" if you want lowercase
                sb.Append(b.ToString("x2"));
            }
 
            return sb.ToString();
        }
    }
Hash("test"); //a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
```
***SHA-1***

**java**
```sh
import java.io.UnsupportedEncodingException; import
java.security.MessageDigest; import
java.security.NoSuchAlgorithmException;

private static String convertToHex(byte[] data) {
StringBuilder buf = new StringBuilder();
for (byte b : data) {
int halfbyte = (b >>> 4) & 0x0F;
int two_halfs = 0;
do {
buf.append((0 <= halfbyte) && (halfbyte <= 9) ? (char) ('0' + halfbyte) : (char) ('a' + (halfbyte - 10)));
halfbyte = b & 0x0F;
} while (two_halfs++ < 1);
}
return buf.toString();
}

public static String SHA1(String text) throws NoSuchAlgorithmException, UnsupportedEncodingException {
MessageDigest md = MessageDigest.getInstance("SHA-1");
byte[] textBytes = text.getBytes("iso-8859-1");
md.update(textBytes, 0, textBytes.length);
byte[] sha1hash = md.digest();
return convertToHex(sha1hash);
}
```
***SHA-2***

**java**
```sh
Java предоставляет встроенный класс MessageDigest для хеширования SHA-256:
MessageDigest digest = MessageDigest.getInstance("SHA-256");
byte[] encodedhash = digest.digest(
originalString.getBytes(StandardCharsets.UTF_8));

Однако здесь мы должны использовать специальный преобразователь байта в шестнадцатеричный, чтобы получить хешированное значение в шестнадцатеричном формате:

private static String bytesToHex(byte[] hash) {
StringBuilder hexString = new StringBuilder(2 * hash.length);
for (int i = 0; i < hash.length; i++) {
String hex = Integer.toHexString(0xff & hash[i]);
if(hex.length() == 1) {
hexString.append('0');
}
hexString.append(hex);
}
return hexString.toString();
}
Нам нужно знать, что MessageDigest не является потокобезопасным . Следовательно, мы должны использовать новый экземпляр для каждого потока.
```
***SHA-3***

**java**
```sh
package com.theromus.sha.example;

import static com.theromus.sha.Parameters.KECCAK_224;
import static com.theromus.sha.Parameters.KECCAK_256;
import static com.theromus.sha.Parameters.KECCAK_384;
import static com.theromus.sha.Parameters.KECCAK_512;
import static com.theromus.sha.Parameters.SHA3_224;
import static com.theromus.sha.Parameters.SHA3_256;
import static com.theromus.sha.Parameters.SHA3_384;
import static com.theromus.sha.Parameters.SHA3_512;
import static com.theromus.sha.Parameters.SHAKE128;
import static com.theromus.sha.Parameters.SHAKE256;
import static com.theromus.utils.HexUtils.convertBytesToString;

import java.nio.charset.StandardCharsets;

import com.theromus.sha.Keccak;

public class KeccakExamples {

    public static void main(String[] args) {
        byte[] data = "The quick brown fox jumps over the lazy dog".getBytes(StandardCharsets.UTF_8);
 
        Keccak keccak = new Keccak();
 
        System.out.println("keccak-224 = " + convertBytesToString(keccak.getHash(data, KECCAK_224)));
        System.out.println("keccak-256 = " + convertBytesToString(keccak.getHash(data, KECCAK_256)));
        System.out.println("keccak-384 = " + convertBytesToString(keccak.getHash(data, KECCAK_384)));
        System.out.println("keccak-512 = " + convertBytesToString(keccak.getHash(data, KECCAK_512)));
 
        System.out.println("sha3-224 = " + convertBytesToString(keccak.getHash(data, SHA3_224)));
        System.out.println("sha3-256 = " + convertBytesToString(keccak.getHash(data, SHA3_256)));
        System.out.println("sha3-384 = " + convertBytesToString(keccak.getHash(data, SHA3_384)));
        System.out.println("sha3-512 = " + convertBytesToString(keccak.getHash(data, SHA3_512)));
 
        System.out.println("shake128 = " + convertBytesToString(keccak.getHash(data, SHAKE128)));
        System.out.println("shake256 = " + convertBytesToString(keccak.getHash(data, SHAKE256)));
    }

}
```
### Список источников
https://datatracker.ietf.org/doc/html/rfc3174

https://tproger.ru/translations/sha-2-step-by-step/

https://www.encryptionconsulting.com/education-center/what-is-sha/

https://ru.wikipedia.org/wiki/%D0%9F%D0%BE%D1%80%D1%8F%D0%B4%D0%BE%D0%BA_%D0%B1%D0%B0%D0%B9%D1%82%D0%BE%D0%B2

https://medium.com/dtechlog/%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC%D1%8B-%D1%85%D1%8D%D1%88-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F-sha-256-9862302f942f

https://proverkassl.com/book_algoritm_sha.html

https://ru.wikipedia.org/wiki/SHA

https://keccak.team/index.html