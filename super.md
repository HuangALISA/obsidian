###### 5.1. Родительский конструктор: super() в constructor()

  
Для того, чтобы вызвать конструктор родительского класса в дочернем классе, следует использовать специальную функцию super(), доступную в конструкторе дочернего класса.  
  
Пусть конструктор ContentWriter вызывает родительский конструктор и инициализирует поле posts:  
  

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
writer.name // Лермонтов
writer.posts // ['Герой нашего времени']
```

  
super(name) в дочернем классе ContentWriter вызывает конструктор родительского класса User.  
  
Обратите внимание, что в дочернем конструкторе перед использованием ключевого слова this вызывается super(). Вызов super() «привязывает» родительский конструктор к экземпляру.  
  

```
class Child extends Parent {
    constructor(value1, value2) {
        // не работает!
        this.prop2 = value2
        super(value1)
    }
}
```

  

###### 5.2. Родительский экземпляр: super в методах

  
Для того, чтобы получить доступ к родительскому методу внутри дочернего класса, следует использовать специальное сокращение super:  
  

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

    getName() {
        const name = super.getName()
        if (name === '') {
            return 'Имярек'
        }
        return name
    }
}

const writer = new ContentWriter('', ['Герой нашего времени'])
writer.getName() // Имярек
```

  
getName() дочернего класса ContentWriter вызывает метод getName() родительского класса User.  
  
Это называется переопределением метода.  
  
Обратите внимание, что super можно использовать и для статических методов родительского класса.