---
title: "[Effective C#] 아이템 41: 값비싼 리소스를 캡처하지 말라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-08-20T12:32:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item41/
---
---

클로저에 캡처된 지역변수는 수명이 늘어나고 선언된 범위를 넘어가도 가비지로 간주되지 않는다.

대부분의 경우 적절한 시점에 가비지로 수집되므로 신경쓸 것이 없다.

그러나 IDisposable을 구현한 무거운 리소스를 참조할 경우 신경써야 할 점들이 있다.

<!--more-->

클로저는 숨겨진 중첩 클래스를 정의한다.

```cs
var counter = 0;
var numbers = Extensions.Generate(30, () => counter++);
```

위 코드는 다음과 같은 코드를 생성한다.

```cs
private class Closure
{
	public int generatedCounter;
	public int generatorFunc() =>
		generatedCounter++;
}
// 사용 예
var c = new Closure();
c.generatedCounter = 0;
var sequence = Extensions.Generate(30, new Func<int>(c.generatorFunc));
```

델리게이트는 숨겨진 클래스 타입의 객체 수명에 영항을 준다.

```cs
public IEnumerable<int> MakeSequence()
{
	var counter = 0;
	var numbers = Extensions.Generate(30, () => counter++);
	return numbers;
}
```

위 코드 처럼 반환 객체가 델리게이트를 사용하면,

델리게이트의 수명이 연장되고 델리게이트가 참조하는 내부 객체, 내부 객체 안에 캡처된 객체도 도달 가능 상태가 된다. (도달 가능한 상태에서는 가비지가 되지 않는다)


대부분의 경우는 문제가 되지 않지만 IDisposable이 연관되어 있는 경우 문제가 될 수 있다.

```cs
public static IEnumerable<string> ReadLines(this TextReader reader)
{
    var txt = reader.ReadLine();

    while (txt != null)
    {
        yield return txt;
        txt = reader.ReadLine();
    }
}

public static int DefaultParse(this string input, int defaultValue)
{
	int answer;

    return (int.TryParse(input, out answer))? answer : defaultValue;
}

public static IEnumerable<IEnumerable<int>>
       ReadNumbersFromStream(TextReader t)
{
    var allLines = from line in t.ReadLines()
                   select line.Split(',');

    var matrixOfValues = from line in allLines
                         select from item in line
                         select item.DefaultParse(0);

	return matrixOfValues;
}
```

위 코드는 CSV 입력 스트림에서 일련의 숫자 시퀀스를 반환하는 코드이다.

세부적인 수행 과정은 중요하지 않고, ReadNumbersFromStream 메서드가 쿼리식을 반환하여 지연 수행된다는 것을 중점적으로 보면 된다.

다음은 위 코드의 사용 예이다.

```cs
var t = new StreamReader(File.OpenRead("TestFile.txt"));
var rowsOfNumbers = ReadNumbersFromStream(t);
```

이 코드는 파일을 닫는 구문이 없어서 문제가 발생할 수 있다.

위 코드를 다음과 같이 변경하여 파일을 닫아주려고 할 수 있다.

```cs
IEnumerable<IEnumerable<int>> rowOfNumbers;
using (TextReader t = new
    StreamReader(File.OpenRead("TestFile.txt")))
    rowOfNumbers = ReadNumbersFromStream(t);
```

하지만 위처럼 코드를 작성하면 제대로 동작하지 않는다. (이후 rowOfNumbers 변수에 접근하려 할 때 ObjectDisposedException이 발생한다)

ReadNumberFromStream 메서드가 지연 수행되므로 실제로 스트림을 읽는 작업은 rowOfNumbers에 접근하는 시점에 수행되기 때문이다.

이 문제는 파일을 닫기 전에 시퀀스를 순회하면 해결할 수 있다.

```cs
 using (TextReader t = new StreamReader(File.OpenRead("TestFile.txt")))
 {
     var arrayOfNums = ReadNumbersFromStream(t);

     foreach (var line in arrayOfNums)
     {
         foreach (var num in line)
             Write("{0}, ", num);
         WriteLine();
     }
 }
```

하지만 항상 위와 같은 방식으로 문제를 해결할 수는 없다.

시퀀스를 순회하는 부분에서 파일이 무조건 열려있어야 하므로 코드가 중복될 여지가 많고 파일을 언제 닫아야 하는지 모호해질 수 있다.

따라서 좀더 일반적인 해결책으로 다음과 같이 ProcessFile() 메서드를 작성할 수 있다.

```cs
// 델리게이트 타입 선언
public delegate TResult ProcessElementsFromFile<TResult>(
    IEnumerable<IEnumerable<int>> values);

// 파일을 읽고, 델리게이트를 이용하여
// 파일의 각 라인들을 처리하는 메서드
public static TResult ProcessFile<TResult>(string filePath,
    ProcessElementsFromFile<TResult> action)
{
    using (TextReader t = new StreamReader(File.Open(filePath)))
    {
        var allLines = from line in t.ReadLines()
                       select line.Split(',');

        var matrixOfValues = from line in allLines
                             select from item in line
                             select item.DefaultParse(0);
                             
        return action(matrixOfValues);
    }
}
```

파일 스트림을 사용하는 코드가 ProcessFile() 메서드 내에 완전히 캡슐화 되었다.

값비싼 리소스(파일 스트림)이 메서드 내에서 할당, 삭제할 수 있고 클로저에 의해 캡처되는 것을 피할 수 있다.

다음 코드는 사용 예시이다.

```cs
// 사용 패턴: 파일과 해당 파일의 각 행별로
// 수행해야 하는 작업을 매개변수로 전달함
ProcessFile("testFile.txt",
    (arrayOfNums) =>
    {
        foreach (var line in arrayOfNums)
        {
            foreach (int num in line)
                Write("{0}, ", num);
            WriteLine();
        }

        return 0;
    }
);

// 예시2: 최대값
var maximum = ProcessFile("testFile.txt",
    (arrayOfNums) => (from line in arrayOfNums
         			  select line.Max()).Max());
```

이처럼 여러 파일 스트림 처리 작업에 대해 ProcessFile() 메서드를 이용할 수 있다.

### 클로저를 사용할 때에 주의해야할 점
컴파일러가 클로저를 구현할 때에는 메서드 내에서 단 하나의 중첩 클래스만을 생성한다.

다음은 잘못된 클로저 사용 예시이다.

```cs
 private static IEnumerable<int> LeakingClosure(int mod)
 {
     var filter = new ResourceHogFilter();
     var source = new CheapNumberGenerator();
     var results = new CheapNumberGenerator();

     var importantStatistic = (from num in source.GetNumbers(50)
                               where filter.PassesFilter(num)
                               select num).Average();

     return from num in results.GetNumbers(100)
            where num > importantStatistic
            select num;
 }
```

importantStatistic이 생성될 때 filter가 클로저에 바인딩 됐다.

그리고 Average() 메서드를 호출하여 전체 시퀀스가 호출되도록 했고, filter가 가비지가 되기를 기대했다.

하지만 마지막에 쿼리문을 반환함으로써 importantStatistic이 클로저에 바인딩 되었고 중첩 클래스의 수명이 연장되었다.

중첩 클래스가 메서드의 범위를 벗어나서도 살아남게 되었고, filter의 수명 또한 연장될 수 있다.


메서드를 2개로 분리하여 2개의 중첩된 클래스를 생성되도록 하여 이 문제를 해결할 수 있다.

```cs
 private static IEnumerable<int> NotLeakingClosure(int mod)
 {
     var importantStatistic = GenerateImportantStatistic();
     var results = new CheapNumberGenerator();
     return from num in results.GetNumbers(100)
            where num > importantStatistic
            select num;
 }

 private static double GenerateImportantStatistic()
 {
     var filter = new ResourceHogFilter();
     var source = new CheapNumberGenerator();
     return (from num in source.GetNumbers(50)
             where filter.PassesFilter(num)
             select num).Average();
 }
```

GenerateImportantStatistic() 메서드 내에 선언되는 중첩 클래스는 메서드 반환시에 즉시 가비지화 된다. (Average() 메서드로 모든 시퀀스를 순회하므로)
