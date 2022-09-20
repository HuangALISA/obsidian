Выражение object instanceof Class определяет, является ли объект экземпляром указанного класса.  
  
Рассмотрим пример:  
  

```
class User {
    name

    constructor(name) {
        this.name = name
    }

    getName() {
        return this.name
    }
}

const user = new User('Печорин')
const obj = {}

user instanceof User // true
obj instanceof User // false
```

  
Оператор instanceof полиморфичен: он исследует всю цепочку классов.  
  

```
class User {
    name

    constructor(name) {
        this.name = name
    }

    getName() {
        return this.name
    }
}

class ContentWriter extends User {
    posts = []

    constructor(name, posts) {
        super(name)
        this.posts = posts
    }
}

const writer = new ContentWriter('Лермонтов', ['Герой нашего времени'])

writer instanceof ContentWriter // true
writer instanceof User // true
```

  
Что если нам нужно определить конкретный класс экземпляра? Для этого можно использовать свойство constructor:  
  

```
writer.constructor === ContentWriter // true
writer.constructor === User // false
// или
writer.__proto__ === ContentWriter.prototype // true
writer.__proto__ === User.prototype // false
```