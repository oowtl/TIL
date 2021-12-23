# Java 23. Map

- Associative array, Hash
- 사전(dict) 과 비슷하다.
   key 와  value 라는 것을 한 쌍으로 가지는 자료형이다.
- 리스트나 배열처럼 순차적으로 해당 요소 값을 구하지 않고 key를 통해서 value 를 얻는다.





## `map.put `

- `put`메소드를 통해서 입력한다.

- ```java
  HashMap<String, String> map = new HashMap<String, String>();
  map.put("people", "사람");
  map.put("baseball", "야구");
  ```



- Map 역시 List와 마찬가지로 인터페이스이다. 
- Map 인터페이스를 구현한 Map자료형에는 `HashMap`, `LinkedHashMap`, `TreeMap`등이 있다.



## `map.get`

- key 에 해당하는 value 값을 얻기 위해서 사용한다.

- ```java
  System.out.println(map.get("people"));
  ```

  - `get`메소드를 사요하면 value 값을 얻을 수 있다.



## `map.containsKey`

- map에 해당하는 key가 있는지를 조사해서 결과값을 리턴한다.

- ```java
  System.out.println(map.containsKey("people")); // true
  System.out.println(map.containsKey("soccer")); // false
  ```



## `map.remove`

- map의 항목을 삭제하는 메소드

- key값에 해당되는 아이템 (key, value)를 삭제한 후에 value 값을 리턴한다.

- ```java
  System.out.println(map.remove("people")); // 사람
  ```





## `map.size`

- map 의 갯수를 리턴한다.

- ```java
  System.out.println(map.size()); // 1
  ```



## map 의 종류

- `HashMap`
  - 순서에 의존하지 않고 key value 로 가져온다.
- `LinkedHashMap`
  - 입력된 순서대로 데이터가 출력된다.
- `TreeMap`
  - 입력된 key 가 정렬도니 순서대로 데이터가 출력된다.



