# [JAVA] Map - getOrDefault 메서드

- 5월 마지막주 수업 리뷰

<br/>

# 🌟 `getOrDefault()` 메서드란?

`getOrDefault()`는 Java에서 Map을 사용할 때, 키 값이 없는 경우를 대비하여 쓰는 메서드

## ✅ 정의

> getOrDefault(Object key, V defaultValue)
>
> - 주어진 key가 Map에 있으면, 그 key에 해당하는 값을 반환하고
> - key가 없으면, 기본값(`defaultValue`)을 반환

## 🔧 언제 쓰나요?

- Map에서 값을 가져올 때 해당 key가 없으면 `null`이 나오는 경우가 있음
- 이럴 때, `null` 대신 미리 정해둔 기본값(=두 번째 인자 값)을 주고 그 값이 나오도록 할 때 사용

## 📌 문법

```java
map.getOrDefault(Object key, V DefaultValue)

map.getOrDefault(찾을_키, 기본값);
```

## 📘 예제

```java
import java.util.HashMap;

public class GetOrDefaultExample {
    public static void main(String[] args) {
        HashMap<String, Integer> scores = new HashMap<>();

        scores.put("영수", 90);  // Key: 영수, Value: 90
        scores.put("철희", 85);    // Key: 철희, Value: 85

        // 존재하는 키인 "영수"의 점수 가져오기
        int score1 = scores.getOrDefault("영수", 50);
        System.out.println("영수의 점수: " + score1);  // 결과: 90

        // Map에 존재하지 않는 키 "지석"의 점수 가져오기 (기본값 50 사용)
        int score2 = scores.getOrDefault("지석", 50);
        System.out.println("지석의 점수: " + score2);  // 결과: 50
    }
}

```

- `"영수"` 는 Map에 있으니까 `90` 을 출력
- `"지석"` 은 Map에 없으니까 기본값 `50` 을 출력

---

## 🎯 비교 정리

- 주로 key 에 대응하는 value 값을 가져올 때, `get` 을 사용한다.

| 메서드                      | 설명                                     |
| --------------------------- | ---------------------------------------- |
| `get(key)`                  | key가 없으면 `null`을 반환               |
| `getOrDefault(key, 기본값)` | key가 없으면 **null 대신 기본값**을 반환 |

---

## 💡 Tip

- `getOrDefault()`를 사용하면 NullPointerException을 피할 수 있다.
- `HashMap에서 동일한 키 값을 추가`할 경우, value의 값이 덮어쓰기가 된다. 따라서 **기존 key 값의 value를 계속 사용하고 싶을 때**, getOrDefault 메서드를 활용할 수 있다.

### 📌 참고한 사이트

https://junghn.tistory.com/entry/JAVA-Map-getOrDefault-%EC%9D%B4%EB%9E%80-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C

https://velog.io/@llocr/Java-Map-getOrDefault-%EB%9E%80

https://gymdev.tistory.com/39
