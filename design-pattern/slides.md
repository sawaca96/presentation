---
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
layout: intro
download: true
---

# 디자인 패턴

디자인 패턴을 사용해 소프트웨어 설계 문제 해결하기

<div class="absolute bottom-10">
  <span class="font-700">
    &copy; sawaca96
  </span>
</div>

<!--
디자인 패턴은 자주 발생하는 소프트웨어 설계 문제에 대한 보편적인 해결책입니다.
개발자라면 유지보수가 쉽고 재사용하기 쉬운 코드를 만들고 싶어합니다. 하지만 실제로 그렇게 하기는 무척 어렵습니다.
따라서 디자인패턴을 통해 더 좋은 설계를 만드는 방법을 배우셨으면 좋겠습니다.
-->

---
layout: center
class: text-center
---

# 시작전에...


<div v-click class="top-50 p-5 font-700">반드시 디자인 패턴을 적용할거야 ❌</div>
<div v-click class="top-80 p-5 font-700">다양한 코딩 노하우를 습득할거야 👌</div>

<!-- 
디자인패턴이 참고할만한 좋은 예시인것은 맞습니다. 하지만 반드시 적용할거라고 생각하면 오류를 범하기 쉽습니다.
디자인패턴을 외우고 반드시 적용하려고 하면 시야가 좁아져 더 좋은 설계를 만들어내지 못합니다. 
따라서 외워서 적용한다기 보단 다양한 코딩 노하우를 습득하려고 하는것이 좋을것 같습니다.
-->

---
layout: center
class: text-center 
---

# 싱글톤 패턴
전역적으로 단 한개의 객체만 사용하자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">


- 한개의 객체만 생성하고 싶을 때            
- 객체의 상태를 공유하고 싶을 때
- 메모리 낭비를 방지하고 싶을 때
</div>

<!-- 
사물실에 한 대의 프린터가 있다고 생각해 봅시다. 해당 프린터를 조작하는 리모컨이 여러개라면 어떻게 될까요?
프린터의 상태를 관리하기가 어렵고 리모컨 낭비가 이루어질것 같습니다.
위와 같은 상황에서 한개의 리모컨만 존재하도록 해주는것이 싱글톤 패턴입니다.
-->

---

# 구조

<div class="m-auto w-2/3">
  <img src="https://refactoring.guru/images/patterns/diagrams/singleton/structure-en-2x.png">
</div>

<!--
싱글톤 패턴에서는 getInstance를 통해서만 객체에 접근할 수 있으며 이때 존재한다면 기존것을 반환하고 없다면 새로 생성합니다. 
파이썬에서는 getInstance를 매직메소드로 대체하여 사용합니다.
-->

---

# 예제

<div v-click-hide>

###### implementation-1
```python
class Singleton(object):
    def __new__(cls):
        if not hasattr(cls, "instance"):
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance
```

###### implementation-2
```python
class MetaSingleton(type):
    _instances = {}

    def __call__(cls, *args, **kwrags):
        if cls not in cls._instances:
            cls._instances[cls] = super(MetaSingleton, cls).__call__(*args, **kwrags)
        return cls._instances[cls]

class Singleton(metaclass=MetaSingleton):
    pass
```
</div>

<div v-after>

###### Result
```python
a = Singleton()
b = Singleton()
print(a is b)
>> True
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
파이썬에서 싱글톤 패턴을 구현하는 방법은 두가지가 있습니다.

첫번째 방법은 new를 오버라이딩해서 사용하고 두번째 방법은 메타클래스를 지정해서 사용합니다.

new는 init보다 먼저 실행되는 메직메소드로써 new에서 메모리할당이 이루어집니다. 따라서 싱글톤 패턴을 구현하기 위해서는 new를 사용해야 합니다.

메타클래스에 대해서 간략하게 설명하자면 메타클래스는 클래스의 타입을 지정하기 위해 사용되는 클래스 입니다.
실제로 기본적인 클래스의 타입은 type이지만 ABC metaclass를 지정할경우 타입이 abc.ABCMeta로 변경되게 됩니다.
보통 클래스의 메직메소드가 다르게 실행되게 하려고 할때 사용합니다.

싱글톤을 구현하기 위해선 call을 오버라이딩 하게 됩니다. 메타클래스를 사용하게 되면 클래스 생성시 메타클래스가 호출되기 때문에 call을 사용하게 됩니다.
 -->

---

# 정리

###### 장점
- 클래스가 하나의 객체만 갖게할 수 있다
- 객체에 접근할 수 있는 방법을 하나로 제한할 수 있다
- 객체의 생성이 단 한번만 이루진다

<br>

###### 단점
- SOLID원칙중 OCP와 DIP를 위반하게 된다
- 객체끼리 서로에 대해 잘 알고 있어야 해서 의존도가 높다
- 멀티쓰레드 환경에서는 인스턴스가 2개 생길 위험이 있다
- 의존도가 높고 mocking이 어려워 테스트가 어렵다

<!-- 
싱글톤 패턴은 가장 유명한 디자인 패턴 이지만 사실은 SOLID를 지키지 않는 안티패턴입니다. 
전역에 걸쳐서 사용되기 때문에 수정사항이 전파되고 있어서 OCP를 어기게 되고 
상위 모듈이 하위 모듈에 의존하지 않고 추상화된 인터페이스에 의존해야 하는데 싱글톤 패턴을 사용할 경우 글로벌 객체에 의존하기 때문에 DIP를 어기게 됩니다.
-->

---
layout: center
class: text-center
---

# 팩토리 패턴

객체의 생성을 책임지는 클래스를 만들자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 객체의 생성과 구현을 나누고 싶을 때 
- 코드를 수정하지 않고 새로운 객체를 추가하고 싶을 때   
</div>

<!-- 
상호의존도를 낮추고 open closed principle을 지킬수 있게 됩니다. 
팩토리 패턴에는 3가지 예시가 있는데 심플 팩토리 패턴, 팩토리 메소드 패턴, 추상 팩토리 패턴 입니다
-->

---

# 예제 - 심플 팩토리 패턴
복수개의 클래스중 한개를 선택할 때 사용한다

<div v-click-hide>

###### implementation
```python
class Animal(metaclass=ABCMeta):
    @abstractmethod
    def do_say(self):
        pass

class Dog(Animal):
    ...

class Cat(Animal):
    ...

class AnimalFactory(object):
    def get_pet(self, pet_type):
        if pet_type == "dog":
            return Dog()
        if pet_type == "cat":
            return Cat()
        else:
            print("sorry, do not have this pet")
```
</div>

<div v-after>

###### result
```python
if __name__ == "__main__":
    factory = AnimalFactory()
    dog = factory.get_pet("dog")
    dog.do_say()

>>> 멍멍
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
사실 패턴이라기 하기엔 조금 별게 없다고 느껴지는 구현입니다. type에 따라서 n개의 객체중 1개를 골라야 할 때 사용하시면 좋을것 같습니다.
이 패턴에서 주의해야 하는점은 Animal이라는 추상 클래스가 존재해서 팩토리에서 나오는 객체가 모두 같은 메소드를 사용한다는 점 입니다.
-->

---

# 구조 - 팩토리 메소드 패턴
여러개의 서브객체를 조합해서 하나의 객체를 생성할 때

<div class="m-auto w-4/5">
  <img src="https://refactoring.guru/images/patterns/diagrams/factory-method/structure-2x.png">
</div>

<!-- 
팩토리 메소드 패턴은 creator와 product가 존재 하는데 creator가 product를 조합한 형태의 객체를 생성하게 됩니다 
-->

--- 


# 예제 - 팩토리 메소드 패턴

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class Section(metaclass=ABCMeta):
    @abstractmethod
    def descried(self):
        pass

class PersonalSection(Section):
    def descried(self):
        print("Personal Section")

class AlbumSection(Section):
    def descried(self):
        print("Album Section")

class PublicationSection(Section):
    def descried(self):
        print("Publication Section")
```
</div>
<div>

###### implementation
```python
class Profile(metaclass=ABCMeta):
    def __init__(self):
        self.sections = []
        self.create_profile()

    @abstractmethod
    def create_profile(self):
        pass

    def get_sections(self):
        return self.sections

    def add_sections(self, section):
        self.sections.append(section)



```
</div>
</div>

<div v-after>

###### implementation
```python
class LinkedIn(Profile):
    def create_profile(self):
        self.add_sections(PersonalSection())
        self.add_sections(PublicationSection())

class Facebook(Profile):
    def create_profile(self):
        self.add_sections(PersonalSection())
        self.add_sections(AlbumSection())
```

</div>
<div v-click>

###### result
```python
liked_in = LinkedIn()
facebook = Facebook()
for section in liked_in.get_sections():
    section.descried()

>>> Personal Section
>>> Publication Section
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
유저의 SNS프로필을 모아 보는 사이트를 생각해봅시다. 
해당 사이트에서 프로필을 추가하는 로직입니다.
여기서 Section이 product이고 Profile이 creator입니다.

팩토리 메소드 패턴은 여러개의 서브 객체를 조합해서 하나의 객체를 생성하게 되는데 
해당 예제를 보시면 Profile이 Section의 조합으로 이루어지고 특정 조합이 특정 SNS프로필을 나타내게 됩니다.
-->

---

# 구조 - 추상 팩토리 패턴
복수개의 성질에 의해 객체의 사용이 구분될 때

<div class="m-auto w-3/4">
  <img src="https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure-2x.png">
</div>

---

# 예제 - 추상 팩토리 패턴

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class PizzaFactory(metaclass=ABCMeta):
    @abstractmethod
    def create_shrimp_pizza(self):
        pass

    @abstractmethod
    def create_cheese_pizza(self):
        pass

class DominoFactory(PizzaFactory):
    def create_shrimp_pizza(self):
        return DominoShrimpPizza()

    def create_cheese_pizza(self):
        return DominoCheesePizza()

class CostcoFactory(PizzaFactory):
    ...





```
</div>
<div>

###### implementation
```python
class ShrimpPizza(metaclass=ABCMeta):
    @abstractmethod
    def prepare(self):
        pass

class CheesePizza(metaclass=ABCMeta):
    @abstractmethod
    def serve(self):
        pass

class DominoShrimpPizza(ShrimpPizza):
    def prepare(self):
        print("Domino shrimp pizza is prepared")

class DominoCheesePizza(CheesePizza):
    def serve(self):
        print("Domino cheese pizza is prepared")

class CostcoShrimpPizza(ShrimpPizza):
    ...
class CostcoCheesePizza(CheesePizza):
    ...
```
</div>
</div>

<div class="grid grid-cols-2 gap-4">
<div v-after>

###### implementation
```python
def get_pizza_factory(factory_type):
    if factory_type == "domino":
        return DominoFactory()
    elif factory_type == "costco":
        return ()

class PizzaFactory:
    def makePizza(self):
        factory = get_pizza_factory("domino")
        domino_shrimp = factory.create_shrimp_pizza()
        domino_shrimp.prepare()
        domino_cheese = factory.create_cheese_pizza()
        domino_cheese.serve()
```
</div>
<div v-click>

###### result
```py
if __name__ == "__main__":
    pizza = PizzaFactory()
    pizza.makePizza()

>>> Domino shrimp pizza is prepared
>>> Domino cheese pizza is prepared








```
</div>
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
피자주문 사이트를 만드는데 피자를 선택할때 고려해야 하는점이 2가지라고 생각해 봅시다.
하나는 피자가게를 고르는것이고 또 하나는 피자종류를 고르는것 입니다.

피자가게를 만들고 피자종류를 생성합니다.
마지막으로 심플 팩토리 패턴에서 사용했던 방법으로 피자가게를 고르고 그 후 피자가게를 통해 피자를 생성하면 구현이 완료됩니다.

결국 저희는 타입을 통해 피자가게를 고르고 해당 피자가게에서 치트피자를 시킬지 새우피자를 시킬지 고르게 됩니다.
-->

---

# 정리

###### 장점
- 생성과 구현로직의 결합력이 낮다
- SRP을 지킬 수 있다
- 쉽게 클래스를 추가할 수 있으므로 OCP를 지킬수 있다

<br>

###### 단점
- 많은 인터페이스와 클래스가 추가됨으로써 복잡해 진다

---
layout: center
class: text-center
---

# 퍼사드 패턴

인터페이스를 제공하자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 복잡한 내부 시스템 로직을 감추기 위해서 
- 클라이언트가 쉽게 시스템에 접근할 수 있는 인터페이스를 제공할 때
</div>

<!-- 어렵게 생각할 필요가 업습니다. 여러가지 메소드로 구성되는 함수를 만드는것이 바로 퍼사드 패턴입니다. 아마 모두가 이미 사용하고 있는 코딩방법일것 같습니다.-->

---

# 구조

<div class="m-auto w-3/4">
  <img src="https://refactoring.guru/images/patterns/diagrams/facade/structure-2x.png">
</div>

<!--
구조를 보시면 client는 facade의 인터페이스를 통해 복잡한 서브클래스들의 로직을 실행하게 됩니다.  
-->

---

### 예제

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class WeddingManager:
    def __init__(self):
        print("WeddingManager:: organize a wedding!")

    def arrange(self):
        self.hotelier = Hotelier()
        self.hotelier.book_hotel()

        self.florist = Florist()
        self.florist.set_flowers()

        self.caterer = Caterer()
        self.caterer.set_menu()

        self.musician = Musician()
        self.musician.set_instruments()






```
</div>
<div>

###### implementation
```python
class Hotelier:
    def book_hotel(self):
        print("Hotelier:: Booking the hotel")

class Florist:
    def set_flowers(self):
        print("Florist:: Picking the flowers")

class Caterer:
    def set_menu(self):
        print("Caterer:: Setting the menu")

class Musician:
    def set_instruments(self):
        print("Musician:: Setting the instruments")

class Person:
    def take_to_wedding(self):
        print("Person:: Taking to the wedding")
        manager = WeddingManager()
        manager.arrange()
```
</div>
</div>

<div v-after>

###### result
```python
if __name__ == "__main__":
    person = Person()
    person.take_to_wedding()


>> Person:: Taking to the wedding
>> WeddingManager:: Let's organize a wedding!
>> Hotelier:: Booking the hotel for the day
>> Florist:: Picking the flowers
>> Caterer:: Setting the menu
>> Musician:: Setting the instruments
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
고객의 웨딩을 책임져주는 웨딩플래너를 제작해봅시다. 고객은 웨딩플래너가 하는 복잡한 일들을 알 필요가 없습니다. 단지 웨딩예약만 하면 됩니다.
facade의 역할을 맡는 웨딩매니저가 호델 꽃 음악과 같은 내부 시스템에 대한 책임을 맡게됩니다.
고객이 해야하는 일은 웨딩매니저의 인터페이스를 이용하는것 뿐입니다.
-->

---
layout: center
class: text-center
---

# 프록시 패턴

기능을 대신 수행하는 비서를 만들자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 객체가 하는 일이 무거워서 매번 실행하거나 생성하기에 부담이 되는 경우 
- 객체의 접근을 통제하여 객체에 대한 보안을 제공해야 하는 경우
</div>

---

# 구조

<div class="m-auto w-1/2">
  <img src="https://refactoring.guru/images/patterns/diagrams/proxy/structure-2x.png">
</div>

<!-- 
client는 동일한 인터페이스를 통해서 real 및 proxy객체를 사용하게 됩니다. 
예를들어 많은 양의 데이터를 받아오는 무거운 작업의 경우 처음엔 real객체를 통해 작업을 진행하고
그 이후부터는 캐시되어있기 때문에 proxy 객체를 통해 작업을 진행할 수 있게 됩니다.

또는 보안검증을 해야하는 경우 항상 proxy객체를 통해서만 real객체에 접근하도록 할 수도 있습니다.
-->

---

### 예제
<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class Thumbnaild(metaclass=ABCMeta):
    @abstractmethod
    def show_name(self):
        pass

    @abstractmethod
    def show_preview():
        pass


class RealThumbnail(Thumbnaild):
    def show_name(self):
        pass

    def show_preview(self):
        print("play video")
```
</div>
<div>

###### implementation
```python
class ProxyThumbnail(Thumbnaild):
    def __init__(self):
        self.status = "DEACTIVATED"
        self.real_thumbnail = RealThumbnail()

    def show_name(self):
        self.real_thumbnail.show_name()

    def show_preview(self):
        if self.status == "ACTIVATED":
            self.real_thumbnail.show_preview()
        else:
            print("show image")

    def activate(self):
        self.status = "ACTIVATED"

```
</div>
</div>

<div v-after>

###### result
```python
if __name__ == "__main__":
    thumb = ProxyThumbnail()
    thumb.activate()
    thumb.show_preview()

>>> show image
>>> play video
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
유튜브의 썸네일을 생각해봅시다.  
유투브의 썸네일은 최초에는 이미지이지만 커서가 올라가면 짧은 영상으로 바뀌게 됩니다.

영상을 보여주는것은 무거운 작업이지만 이미지를 보여주는것은 보다 가벼운 작업이기 때문에 proxy객체가 이미지를 보여주고 
커서가 올라왔을 때만 real객체를 통해 영상을 보여주도록 합니다.
-->

---

# 정리

###### 장점
- 서비스객체의 무거운 작업을 매번 실행할 필요가 없어진다
- 서비스객체를 수정하지 않고 새로운 proxy를 추가할 수 있다

<br>

###### 단점
- 요청/반환이 proxy를 거치기 때문에 성능저하가 일어날 수 있다

---
layout: center
class: text-center
---

# 옵저버 패턴

이벤트로 소통하자 

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 객체의 상태를 다른 객체에 알려야 할 때
- 발행/구독 모델이 필요할 때
- 일대다 관계에서 객체간의 관계를 느슨하게 구성하고 싶을 때
</div>

---

# 구조

<div class="m-auto w-4/5">
  <img src="https://refactoring.guru/images/patterns/diagrams/observer/structure-2x.png">
</div>

<!-- 
publisher와 subscriber가 존재하며 publisher는 subscribe목록을 들고 있게 됩니다.
추후 이벤트가 발생하면 subscriber들에게 이벤트를 전송하게 됩니다.

이렇게 구성하게 되면 Publisher는 옵저버가 어떤 로직으로 구현됐는지 알 필요가 없습니다.
서로 의존적이지 않고 느슨하게 결합되어 있기때문에 유지 보수 및 새로운 옵저버의 추가가 쉽습니다. 
-->

---

### 예제

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class NewsPublisher:
    def __init__(self):
        self._subscribers = []

    def attch(self, subscriber):
        self._subscribers.append(subscriber)

    def detach(self, subscriber):
        self._subscribers.remove(subscriber)

    @property
    def subscribers(self):
        return self._subscribers

    def notify(self, news):
        for subscriber in self._subscribers:
            subscriber.update(news)






```
</div>
<div>

###### implementation
```python
class Subscriber(ABC):
    @abstractmethod
    def update(self):
        pass

class SMSSubscriber(Subscriber):
    def __init__(self, publisher):
        self.publisher = publisher
        self.publisher.attch(self)

    def update(self, news):
        name = type(self).__name__
        print(f"{name} received: {news}")

class EmailSubscriber(Subscriber):
    def __init__(self, publisher):
        self.publisher = publisher
        self.publisher.attch(self)

    def update(self, news):
        name = type(self).__name__
        print(f"{name} received: {news}")
```

</div>
</div>

<div v-after>

###### result
```python
if __name__ == "__main__":
    news_publisher = NewsPublisher()
    sms_subscriber = SMSSubscriber(news_publisher)
    email_subscriber = EmailSubscriber(news_publisher)

    news_publisher.notify("News 1")

    news_publisher.detach(sms_subscriber)
    news_publisher.notify("News 2")

>>> SMSSubscriber received: News 1
>>> EmailSubscriber received: News 1
>>> EmailSubscriber received: News 2
```

</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!--
뉴스 구독 시스템을 설계해봅시다.

뉴스를 구독할 때 문자구독과 이메일 구독 기능이 존재합니다.
 
-->

---

# 정리

###### 장점
- 이벤트를 활용하는 새로운 객체를 추가하기가 쉽다
- 필요한 경우 런타임중에 subscriber를 추가할 수 있다

<br>

###### 단점
- 이벤트의 순서가 올바르지 않을 수 있다(Race Condition)
- 이벤트가 반드시 전송되는것을 보장하기 어렵다

<br>

###### 풀 vs 푸시
- 풀 : 알리는 단계와 받아오는 단계가 있어 비효율적이다
- 푸시 : 불필요한 데이터도 보낼 수 있으며 많은 양의 데이터를 전송해 응답 시간이 늦어질 수 있다

---
layout: center
class: text-center
---

# 템플릿 메소드 패턴

알고리즘 구성을 템플릿으로 만들어 놓자

---
layout: center
---


# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 클래스끼리 비슷하거나 같은 로직을 구현할 때
- 비슷한 클래스를 여러개 생성하는 경우 
</div>

---

# 구조

<div class="m-auto w-3/7">
  <img src="https://refactoring.guru/images/patterns/diagrams/template-method/structure-2x.png">
</div>

<!--  
예를 들면 차를 끓이는 알고리즘과 커피를 끓이는 알고리즘과 같은 경우입니다. 

이렇게 하면 코드 중복을 줄일 수 있습니다.
-->

---

### 예제

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class Trip(metaclass=ABCMeta):
    def set_transport(self):
        print("Bus")

    @abstractmethod
    def day_1(self):
        pass

    @abstractmethod
    def day_2(self):
        pass

    def go_trip(self):
        self.set_transport()
        self.day_1()
        self.day_2()
        self.go_home()
```
</div>
<div>

###### implementation
```python
class UnistTrip(Trip):
    def day_1(self):
        print("Unist day 1")

    def day_2(self):
        print("Unist day 2")


class SeoulTrip(Trip):
    def day_1(self):
        print("Seoul day 1")

    def day_2(self):
        print("Seoul day 2")




```

</div>
</div>

<div v-after>

###### result
```python
if __name__ == "__main__":
    unist = UnistTrip()
    unist.go_trip()

    seoul = SeoulTrip()
    seoul.go_trip()

>>> Bus
>>> Unist day 1
>>> Unist day 2

>>> Bus
>>> Seoul day 1
>>> Seoul day 2
```

</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!--  
여행상품을 판매하는 업체를 생각해 봅시다.
해당 업체는 유니스트 여행과 서울 여행을 판매하고 있습니다.

템플릿을 사용함으로써 버스를 사용하는 코드의 중복을 줄일 수 있었습니다.
-->

---

# 정리

###### 장점
- 추상화클래스의 일부분만 오버라이딩해서 새로운 알고리즘을 만들 수 있다
- 코드중복을 방지할 수 있다

<br>

###### 단점
- 템플릿이 알고리즘 생성을 제한할 수 있다
- LSP(Liskov Substitution Principle)을 위반하게 된다
- 템플릿의 단계가 많으면 유지보수가 어려워진다

<!-- 
템플릿을 상속받은 알고리즘의 경우 오버라이딩해서 로직이 변경할 수 있습니다. 
이때 오버라이딩한 메소드는 부모 메소드로 치환이 안되기 때문에 리스코브 치환원칙을 어기게 됩니다  
-->

---
layout: center
class: text-center
---

# 커맨드 패턴

요청을 캡슐화하자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 명령을 객체화 해서 사용할 때
- 요청을 실행시키는 시점을 조정하고 싶을 때
- 요청을 취소시켜야 하는 기능이 필요할 때
</div>

<!-- 
웹 페이지에 수많은 버튼이 있을때 각 버튼이 비지니스 로직을 갖고 있으면 유지보수가 힘들어 집니다.
하지만 각 버튼이 클릭될때 특정 명령어 객체를 실행하면 비즈니스 로직이 분리할 수 있게 됩니다.
-->

---

# 구조

<div class="m-auto w-4/5">
  <img src="https://refactoring.guru/images/patterns/diagrams/command/structure-2x.png">
</div>

<!--
client command receiver invoker로 구성됩니다.

receiver는 비지니스 로직을 들고 있는 객체로써 커맨드의 실행시 사용할 로직을 들고 있습니다.
command는 추상화된 객체로써 receiver를 입력받아 invoker에게 동일한 인터페이스를 제공합니다.
invoker는 command를 모아두고 있다가 client가 원할 때 command들을 실행합니다.
-->

---

### 예제

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class Order(metaclass=ABCMeta):
    @abstractmethod
    def execute(self):
        pass

class BuyStockOrder(Order):
    def __init__(self, stock):
        self.stock = stock

    def execute(self):
        self.stock.buy()

class SellStockOrder(Order):
    def __init__(self, stock):
        self.stock = stock

    def execute(self):
        self.stock.sell()



```
</div>
<div>

###### implementation
```python
class StokTrade:
    def buy(self):
        print("Buy stock")

    def sell(self):
        print("Sell stock")

class Agent:
    def __init__(self):
        self._order_queue = []

    def place_order(self, order):
        self._order_queue.append(order)
    
    def send_order(self):
        for order in self._order_queue:
            order.execute()





```

</div>
</div>

<div v-after>

###### result
```py
if __name__ == "__main__":
    stock = StokTrade()
    agent = Agent()
    agent.place_order(BuyStockOrder(stock))
    agent.place_order(SellStockOrder(stock))

>>> Buy stock
>>> Sell stock
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
주식거래를 생각해 봅시다. 
명령어로는 매수와 매도가 존재합니다. 매수와 매도는 StockTrade를 통해 실제 매수/매도를 진행하게 됩니다.
Agent invoker는 명령어를 담고 실행할 수 있게 해줍니다.
여기서 Receiver는 StockTrade Invoker가 Agent Command는 Order입니다. 
-->

---

# 정리

###### 장점
- SRP를 지키게 된다
- OCP를 지키게 된다
- 취소기능을 구현할 수 있다
- 실제 명령어가 실행되는 시점을 늦출 수 있다

<br>

###### 단점
- 서비스가 복잡해진다

---
layout: center
class: text-center
---

# 전략 패턴

사용하는 알고리즘을 변경하자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 특정 컨텍스트에 따라 로직을 변경해야 할 때
- 런타임중에 기능의 로직을 변경해야 할 때
</div>

<!-- 검색창을 구현할 때 선택한 섹터에 따라 다른 검색기능을 제공하는 경우와 같은 상황에 사용됩니다. -->

---

# 구조

<div class="m-auto w-3/5">
  <img src="https://refactoring.guru/images/patterns/diagrams/strategy/structure-2x.png">
</div>

---

### 예제

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class Context:
    def __init__(self, strategy: Strategy) -> None:
        self._strategy = strategy

    @property
    def strategy(self) -> Strategy:
        return self._strategy

    @strategy.setter
    def strategy(self, strategy: Strategy) -> None:
        self._strategy = strategy

    def do_some_business_logic(self) -> None:
        print("Context: Sorting data")
        result = self._strategy.do_algorithm(
            ["a", "b", "c", "d", "e"]
        )
        print(",".join(result))
```
</div>
<div>

###### implementation
```python
class Strategy(ABC):
    @abstractmethod
    def do_algorithm(self, data: List):
        pass


class ConcreteStrategyA(Strategy):
    def do_algorithm(self, data: List) -> List:
        return sorted(data)


class ConcreteStrategyB(Strategy):
    def do_algorithm(self, data: List) -> List:
        return reversed(sorted(data))





```

</div>
</div>

<div v-after>

###### result
```python
if __name__ == "__main__":
    context = Context(ConcreteStrategyA())
    print("Client: Strategy is set to normal sorting.")
    context.do_some_business_logic()

    print("Client: Strategy is set to reverse sorting.")
    context.strategy = ConcreteStrategyB()
    context.do_some_business_logic()

>>> Client: Strategy is set to normal sorting.
>>> Context: Sorting data
>>> a,b,c,d,e

>>> Client: Strategy is set to reverse sorting.
>>> Context: Sorting data
>>> e,d,c,b,a
```

</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!-- 
컨텍스트 객체는 런타임중 상황에 따라 알고리즘을 변경하게 됩니다.
-->

---

# 정리

<div class="grid grid-cols-2 gap-4">

<div>

###### 템플릿 패턴과 차이점
- 템플릿 패턴은 상속으로 이루어지고 전략 패턴은 컴포지션으로 이루어진다
- 템플릿 패턴은 컴파일 단계에서 행동이 결정되고 전략 패턴은 런타임중에 결정된다

<br>

###### 커맨드 패턴과 차이점
- 커맨드 패턴은 '어떻게'를 다루고 / 전략 패턴은 '무엇'을 다룬다
- 커맨드 패턴은 작업을 객체화 해서 defer execution, store hisotry와 같은 기능을 구현한다
- 전략 패턴은 같은것을 다른 방법으로 진행할 때 사용한다
</div>

<div>

###### 장점
- 알고리즘을 런타임중에 변경할 수 있다
- 상속대신 구성을 사용할 수 있다
- OCP를 지키게 되서 알고리즘 추가가 쉽다

<br>
<br>

###### 단점 
- 알고리즘의 개수나 로직이 거의 변경되지 않는 경우 오버스펙이 될 수 있다
- anonymous functions를 통해서도 구현할 수 있다(lamda function)
</div>

</div>

<!-- 체육시간에 농구 축구 야구중 무엇을 할지 정하는것은 전략 패턴이고 축구를 하는데 패스할지 슛할지 태클을 할지 정하는것은 커맨트 패턴이라고 생각하시면 될 것 같습니다 -->

---
layout: center
class: text-center
---

# 상태 패턴

객체의 상태를 변경하자

---
layout: center
---

# 언제 사용할까?

<div class="border-t border-gray-400 border-opacity-25 !all:leading-12 !all:list-disc">

- 상태에 따라 알고리즘이 변경 될 때 
- 객체의 상태가 서로 영향을 받을 때
- 객체 상태에 대한 고츅이 유한하고 미리 정해져 있을 때
</div>

<!-- 라디오의 AM/FM 채널을 선택하는 상황이 상태 패턴이라고 볼 수 있습니다. 객체가 갖는 상태가 정해져 있고 상태에 따라 하는 행위가 변경되는 경우입니다. -->

---

# 구조

<div class="m-auto w-3/5">
  <img src="https://refactoring.guru/images/patterns/diagrams/state/structure-en-2x.png">
</div>

<!-- 
Context라는 객체가 존재하며 해당 객체가 State를 변경해가며 state가 갖고 있는 로직을 실행하게 됩니다.
-->

---

### 예제

<div v-click-hide class="grid grid-cols-2 gap-4">
<div>

###### implementation
```python
class ComputerState(ABC):
    name = "state"
    allowed = []

    def switch(self, state):
        if state.name in self.allowed:
            print(f"{self.name} ➡️ {state.name} 👌")
            self.__class__ = state
        else:
            print(f"{self.name} ➡️ {state.name} ❌")
    
    def __str__(self):
        return self.name







```
</div>
<div>

###### implementation
```python
class ComputerOff(ComputerState):
    name = "OFF"
    allowed = ["on"]

class ComputerOn(ComputerState):
    name = "ON"
    allowed = ["off", "suspended"]

class ComputerSuspended(ComputerState):
    name = "SUSPENDED"
    allowed = ["on"]

class Computer:
    def __init__(self):
        self.state = ComputerOff()
    
    def switch(self, state):
        self.state.switch(state)


```

</div>
</div>

<div v-after>

###### result
```py
if __name__ == "__main__":
    c = Computer()
    c.switch(ComputerOn)
    c.switch(ComputerOff)
    c.switch(ComputerSuspended)

>>> OFF ➡️ ON 👌
>>> ON ➡️ OFF 👌
>>> OFF ➡️ SUSPENDED ❌
```
</div>

<style>
    .slidev-vclick-hidden{
        display:none
}
</style>

<!--  
컴퓨터의 전원을 상상해 봅시다.
컴퓨터는 ON OFF SUSPENDED상태가 존재하며 OFF에서 SUSPENDED로 갈수는 없습니다.
-->

---

# 정리
###### 전략 패턴과 차이점
- 상태패턴은 전략패턴의 연장선이다
- 전략패턴은 컨텍스트가 서로 의존적이지 않지만 상태패턴은 서로의 상태에 따라 기능이 제한될 수 있다

<br>

###### 장점
- SRP 및 OCP를 지키게 된다

<br>

##### 단점
- 상태변경이 거의 없는경우 패턴 적용이 무의미 하다
