# 빌더 패턴

## 구현 방법
1. 정적 내부 클래스의 인스턴스 생성

2. 내부 클래스 인스턴스에 setter로 필요한 값을 넣는다.

3. build 메서드를 통해 파라미터로 인스턴스를 넘겨 상위 클래스의 생성자를 호출하고, 넣은 값만으로 상위 클래스의 인스턴스를 생성한다.

# 1. 롬복 스타일
```java
class Pasta {
    private String noodle;
    private String source;
    private String vegetables;

    private Pasta(Builder builder) {
        this.noodle = builder.noodle;
        this.source = builder.source;
        this.vegetables = builder.vegetables;
    }

    // 정적 팩토리 메서드 - builder()
    public static Builder builder() {
        return new Builder();
    }

    public static class Builder {
        private String noodle;
        private String source;
        private String vegetables;

        private Builder() {}

        public Builder noodle(String noodle) {
            this.noodle = noodle;
            return this;
        }

        public Builder source(String source) {
            this.source = source;
            return this;
        }

        public Builder vegetables(String vegetables) {
            this.vegetables = vegetables;
            return this;
        }

        public Pasta build() {
            return new Pasta(this);
        }
    }
}



class Main {
    public static void main(String[] args) {

        Pasta pasta = Pasta.builder()
                            .noodle("noodle")
                            .source("tomato")
                            .vegetables("mushroom")
                            .build();
    }
}
```

# 2. new 키워드 스타일
```java
class Pasta {
    private String noodle;
    private String source;
    private String vegetables;

    private Pasta(Builder builder) {
        this.noodle = builder.noodle;
        this.source = builder.source;
        this.vegetables = builder.vegetables;
    }

    public static class Builder {
        private String noodle;
        private String source;
        private String vegetables;

        public Builder() {}

        public Builder noodle(String noodle) {
            this.noodle = noodle;
            return this;
        }

        public Builder source(String source) {
            this.source = source;
            return this;
        }

        public Builder vegetables(String vegetables) {
            this.vegetables = vegetables;
            return this;
        }

        public Pasta build() {
            return new Pasta(this);
        }
    }
}



class Main {
    public static void main(String[] args) {
        Pasta pasta = new Pasta.Builder()
                                .noodle("noodle")
                                .source("tomato")
                                .vegetables("mushroom")
                                .build();
    }
}
```