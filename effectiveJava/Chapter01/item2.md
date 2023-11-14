# item2. 생성자에 매개변수가 많다면 Builder를 고려하라

```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // 필수 매개변수
        private final int servingSize;
        private final int servings;

        // 선택 매개변수 - 기본값으로 초기화
        private final int calories;
        private final int fat;
        private final int sodium;
        private final int carbohydrate;

        // 빌더의 생성자 메서드
        public Builder(int servingSize, int servings){
            this.servingSize = servingSize;
            this.servings = servings;
        }

        // 빌더 객체를 반환하는 setter 메서드들
        public Builder calories(int val){
            calories = val; 
            return this;
        }
        public Builder fat(int val){
            fat = val; 
            return this;
        }
        public Builder sodium(int val){
            sodium = val; 
            return this;
        }
        public Builder carbohydrate(int val){
            carbohydrate = val; 
            return this;
        }

        public NutritionFacts build(){
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder){
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }

    public static void main(String[] args){
        NutritionFacts cocaCola = new NutritionFacts
        .Builder(240,8)
        .calories(100)
        .sodium(35)
        .carbohydrate(27)
        .build();
    }
}
```

빌더의 세터 메서드들은 빌더 자신을 반환하기 때문에 연쇄적으로 호출할 수 있다.
이런 방식을 메서드 호출이 흐르듯 연결된다는 뜻으로 플루언트 API 혹은 메서드 연쇄라 한다.

