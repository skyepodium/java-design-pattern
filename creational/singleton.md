# 싱글톤 패턴

생성 방법 6가지 중 권장되는 방법은 `1. Lazy Holder, 2. enum` 2가지입니다.

자세한 내용은 블로그에 정리했습니다.    

[[자바] 싱글톤 정말 한개만 만들어봅시다.](https://velog.io/@skyepodium/%EC%8B%B1%EA%B8%80%ED%86%A4-%EC%A0%95%EB%A7%90-%ED%95%9C%EA%B0%9C%EB%A7%8C-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B4%85%EC%8B%9C%EB%8B%A4)

[클래스는 언제 로딩되고 초기화되는가? (feat. 싱글톤)](https://velog.io/@skyepodium/%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-%EC%96%B8%EC%A0%9C-%EB%A1%9C%EB%94%A9%EB%90%98%EA%B3%A0-%EC%B4%88%EA%B8%B0%ED%99%94%EB%90%98%EB%8A%94%EA%B0%80)

# 1. private 생성자
```java
public class Settings {
    // 1. 클래스 변수로 인스턴스 지정
    private static Settings instance;

    // 2. private 생성자 - new로 인스턴스 생성불가
    private Settings() {}

    // 3. 클래스 메서드를 통해 인스턴스 생성
    public static Settings getInstance() {
        // 주의 - 여러개의 스레드가 동시에 if문을 통과하는 경우 여러개의 인스턴스 생성 가능
        if (instance == null) {
            instance = new Settings();
        }

        return instance;
    }
}
```

# 2. synchronized
```java
public class Settings {
    // 1. 클래스 변수로 인스턴스 지정
    private static Settings instance;

    // 2. private 생성자 - new로 인스턴스 생성불가
    private Settings() {}

    // 3. 클래스 메서드를 통해 인스턴스 생성
    public static synchronized Settings getInstance() {
        // 주의 - 여러개의 스레드가 동시에 if문을 통과하는 경우 여러개의 인스턴스 생성 가능
        if (instance == null) {
            instance = new Settings();
        }

        return instance;
    }
}
```

# 3. Eager initialization
```java
public class Settings {
    // 1. 클래스가 로드되는 시점에 인스턴스를 생성
    private static final Settings INSTANCE = new Settings();

    // 2. private 생성자
    private Settings() {}

    // 3. 클래스 변수를 통한 인스턴스 획득
    public static Settings getInstance() {
        return INSTANCE;
    }
}
```

# 4. double checked locking
```java
public class Settings {
    // 1. 변수를 메인 메모리에 저장
    private static volatile Settings instance;

    // 2. private 생성자
    private Settings() {}

    // 3. getInstance 메서드 호출될때는 lock 걸리지 않음
    public static Settings getInstance() {
        if (instance == null) {
            synchronized (Settings.class) {
                if (instance == null) {
                    instance = new Settings();
                }
            }
        }
        return instance;
    }
}
```

# 5. Lazy Holder
```java
public class Settings {
    // 1. private 생성자
    private Settings() {}

    // 2. static inner 클래스
    private static class SettingsHolder {
        private static final Settings INSTANCE = new Settings();
    }

    // 3. static 메서드
    public static Settings getInstance() {
        return SettingsHolder.INSTANCE;
    }
}
```

# 6. enum
```java
public enum Settings {

    INSTANCE;
}
```