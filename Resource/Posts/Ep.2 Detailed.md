

# 어쩌다 문제를 만나게 되었나

이 연구 또한 내 이전연구 (컨텍스트 터널링) [2]와 같이 간단한 예시에서 출발한다.
%
자세한 설명은 하지 않겠지만, 컨텍스트 터널링[2]은 함수 호출 요약을 더 잘 할 수 있게 해주는 기술로써 위치 기반 함수 호출 요약 (call-site sensitivity)과 값 기반 함수 호출 요약 (object sensitivity)의 성능을 모두 올릴 수 있는 기술이다.
%
예를 들어, 아래의 예제는 정적 분석 교과서[4]에서 1-값 기반 함수 호출 요약(1-object sensitivity)의 장점 및1-위치 기반 함수 호출 요약(1-call-site sensitivity)의 단점을 설명하기 위해 사용한 예시인데, 컨텍스트 터널링을 사용하면 1-위치 기반 함수 호출 요약으로도 아래 예제를 정확하게 분석할 수 있다.
%

```java
class S {
  Object id(Object a){ return a; }
  Object id2(Object a){ return a; }
}
class C extends S {
  void fun1() { 
    Object a1 = new A1(); 
    Object b1 = id2(a1);
  }
}
class D extends S {
  void fun2() {
    Object a2 = new A2();
    Object b2 = id2(a2);
  }
}
```

%
즉, 더이상 위 예제는 더이상 값 기반 함수 호출 요약 대비 위치 기반 함수 호출 요약의 약점을 보여주는 예시가 아니게 된다. 


이 후 컨텍스트 터널링이 사용되었을 때 위치 기반 함수 호출 요약의 약점을 보여주는 예시를 만들고자 하였다.
%
<br>
<br>

...
<br>
<br>

아무리 고민해봐도 안만들어진다.
%
일주일 동안 열심히 고민해 보았으나 결국 예제를 만드는 것에 실패하였다.
%
반면, 반대의 경우 (값 기반 함수 호출 요약의 약점)를 보여주는 예제는 쉽게 만들어졌다.

```java
class D {
  Object id(Object a){ return a; }
}

main() { 
  Object d = new D(); 
  A a = (A) d.id(new A());
  B a = (B) d.id(new B());
  C a = (C) d.id(new C());	
  }
}
```

자세히 설명하진 않겠지만 위 예시는 값 기반 함수 호출 요약(object sensitivity)으로는 정확하게 분석하는 것이 불가능하다 (자세한 내용은 논문[3]을 참고해주실 바란다.)
%
결국 위 경험들로 부터 아래와 같은 가설에 도달하게 되었다.

> 컨텍스트 터널링이 들어간 경우 위치 기반 함수 호출 요약(call-site sensitivity)이 값 기반 함수 호출 요약(object sensitivity)보다 더 정확하다.

이 주제 연구해보겠다고 지도교수님에게 처음 제안 하였을 때 매우 의심스러워 하셨던 것이 기억이 난다.
%
한 가지 기억나는 코멘트들을 적어보자면.
%
> "민석이가 반례를 못만들면 다른 사람들도 못만드는 거니?"

당시에는 "그렇진 않죠..." 하며 얼버무리고 나왔었다.
%
이 후 충분한 실험적 증거들을 보여드리며 설득해 나갔었다.


## 어떻게 이 문제를 풀게 되었나 

컨텍스트 터널링이 있을 때 위치 기반 함수 호출 요약(call-site sensitivity)이 값 기반 함수 호출 요약(object sensitivity)보다 더 정확하다는 것을 보이는 방법은 두가지가 있다.

1. 위치 기반 요약의 최대 정확도가 값 기반 요약의 최대 정확도보다 높음을 보이기.
2. 값 기반 함수 호출 요약을 더 정확한 위치 기반 함수 호출 요약으로 변환시켜주는 함수 존재함을 보이기.

내 선택은 2번이었고, 이론적으로는 보장되지 않지만 실험적으로는 값 기반 함수 호출 요약을 더 정확한 위치 기반 함수 호출 요약으로 바꿔주는 함수를 만들었다.







## References

[1] Tan, T., Li, Y., Xue, J. (2016). Making _k_-Object-Sensitive Pointer Analysis More Precise with Still _k_-Limiting. In: Rival, X. (eds) Static Analysis. SAS 2016. Lecture Notes in Computer Science(), vol 9837. Springer, Berlin, Heidelberg. https://doi.org/10.1007/978-3-662-53413-7_24

[2] Minseok Jeon, Sehun Jeong, and Hakjoo Oh. 2018. Precise and scalable points-to analysis via data-driven context tunneling. Proc. ACM Program. Lang. 2, OOPSLA, Article 140 (November 2018), 29 pages. https://doi.org/10.1145/3276510

[3] Minseok Jeon and Hakjoo Oh. 2022. Return of CFA: call-site sensitivity can be superior to object sensitivity even for object-oriented programs. Proc. ACM Program. Lang. 6, POPL, Article 58 (January 2022), 29 pages. https://doi.org/10.1145/3498720

[4] Yannis Smaragdakis and George Balatsouras (2015), "Pointer Analysis", Foundations and Trends® in Programming Languages: Vol. 2: No. 1, pp 1-69. http://dx.doi.org/10.1561/2500000014