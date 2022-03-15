# 빌더 패턴

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