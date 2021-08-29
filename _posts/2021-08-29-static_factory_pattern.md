---
layout: single
title:  static factory_pattern
---


주의 : 디자인 패턴의 팩터리 메서드와 다르다.

``` java
class Character {

    int intelligence, strength, hitPoint, magicPoint;

    public Character(int intelligence, int strength, int hitPoint, int magicPoint) {
        this.intelligence = intelligence;   // 지능
        this.strength = strength;           // 힘
        this.hitPoint = hitPoint;           // HP
        this.magicPoint = magicPoint;       // MP
    }

    // 정적 팩토리 메소드
    public static Character newWarrior() {
        return new Character(5, 15, 20, 3);     // 전사는 힘과 HP가 높다
    }

    // 정적 팩토리 메소드
    public static Character newMage() {
        return new Character(15, 5, 10, 15);    // 마법사는 지능과 MP가 높다
    }
}
```
1–1 정적 팩터리 메서드의 사용목적
객체 생성을 캡슐화하는 기법
1–2 정적 팩터리 메서드의 장점
public 생성자와 달리 이름을 가질 수 있다
코드를 읽는 개발자가 어떤 역할을 하는지 정확히 알 수 있다.(가독성)

``` java
public class ContactMain {

    public static void main(String[] args) {

        //어떤 내용을 담고있는 객체를 만들지 이름만봐서 알수 있다
        Contact contactEmpty = Contact.newEmptyInstance();

        Contact contactName = Contact.newInstanceByName("ifContinue");

        Contact contactNamePhoneNum = Contact.newInstanceByNamePhoneNum("ifContinue", 123456789);



        System.out.println("contact empty : " + contactEmpty.getName() + " / " + contactEmpty.getPhoneNum());

        System.out.println("contact name : " + contactName.getName() + " / " + contactName.getPhoneNum());

        System.out.println("contact name and phonenum : " + contactNamePhoneNum.getName() + " / " + contactNamePhoneNum.getPhoneNum());

    }
```

``` java
static class Contact {

    private static Contact contact;
    private String name;
    private int phoneNum;

    //private 형태의 constructor : 바로 클래스에 접근해서 객체생성 불가능
    private Contact () {}

    private Contact(String name) {
        this.name = name;
    }

    private Contact(String name, int phoneNum) {
        this.name = name;
        this.phoneNum = phoneNum;
    }

    //static method를 통해서만 객체 생성 가능

    public static Contact newEmptyInstance(){
        contact = new Contact();
        return contact;
    }


    public static Contact newInstanceByName(String name){
        contact = new Contact(name);
        return contact;

    }

    public static Contact newInstanceByNamePhoneNum(String name, int phoneNum) {
        contact = new Contact(name, phoneNum);
        return contact;
    }


    //getter and setter

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getPhoneNum() {
        return phoneNum;
    }

    public void setPhoneNum(int phoneNum) {
        this.phoneNum = phoneNum;
    }
}
}
```
호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.
인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용 할 수 있다.
그렇기 때문에 객체가 자주 요청되는 상황이라면 성능을 상당히 끌어올려 준다.
인스턴스 통제 → 싱글턴, 인스턴스화 불가로 만들 수 있음
반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
서비스 제공자 프레임워크(service provider framework)를 만드는 근간
ex) JDBC
1–3 정적 팩터리 메서드의 단점
상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
2. 정적 팩터리 메서드의 명명방식
   
   from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
   Date date = Date.from(instant);
   
   of : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
   Set<Rank> faceCards = EnumSet.of(JACK,QUEEN,KING);
   
   valueOf : from과 of의 더 자세한 버전
   BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
   
   instance 혹은 getInstance : 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
   StackWalker luke = StackWalker.getInstance(options);
   create 혹은 newInstance : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장함
   
   Object newArray = Array.newInstance(classObject, arrayLen);
   getType : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. “Type”은 팩터리 메서드가 반환할 객체의 타입
   
   FileStore fileStore = Files.getFileStore(path);
   newType : newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. “Type”은 팩터리 메서드가 반환할 객체의 타입
   
   BufferedReader bufferedReader = Files.newBufferedReader(path);
   type : getType 과 newType의 간결한 버전
   List<Info> infoList = Collections.list(informations);