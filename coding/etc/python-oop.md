# 1. Pendahuluan OOP
- Programming Paradigma(Cara berfikir) :
    1. Structural (Procedural Programming)
    - ciri-ciri :
        1. diekesekusi berdasarkan urutan
    2. OOP
    - ciri-ciri :
        1. semua dianggap object    
        2. class memiliki attribute dan method
    - exp :
        ```python
        class Hero: #template
            pass


        hero1 = Hero() #object / instance

        hero1.name = "sniper"
        hero1.health = 100

        print(hero1) # <__main__.Hero object at 0x7ff7e25135f8>
        print(hero1.__dict__) # {'name': 'sniper', 'health': 100}
        print(hero1.name) # sniper
        ```

# 2. Constructor `__init__()`
```python
class Hero: #template
    def __init__(self, name, health): # yang pertama dijalankan ketika object dibuat
        self.name = name
        self.health = health


hero1 = Hero("lina", 1350) #object / instance

print(hero1) # <__main__.Hero object at 0x7f1ba7b955f8>
print(hero1.__dict__) # {'name': 'lina', 'health': 1350}
print(hero1.name) # lina
print(hero1.health) # 1350
```

# 3. Class dan Instance Variable
```python
class Hero: #template
    # class variable (nempel di class)
    jumlah = 0
    def __init__(self, name, health): # yang pertama dijalankan ketika object dibuat
        # instance variable (nempel di object)
        self.name = name
        self.health = health
        Hero.jumlah += 1


hero1 = Hero("lina", 1350)
print(Hero.jumlah) # 1
hero1 = Hero("sniper", 1150) #object / instance
print(Hero.jumlah) # 2
```

# 4. Method
```python
class Hero: #template
    # class variable (nempel di class)
    jumlah = 0
    def __init__(self, name, health): # yang pertama dijalankan ketika object dibuat
        # instance variable (nempel di object)
        self.name = name
        self.health = health
        Hero.jumlah += 1
    
    # method tanpa return
    def siapa(self):
        print("namaku adalah " + self.name)
    
    # method dengan argumen 
    def healthUp(self, up):
        self.health += up
    
    # method dengan return
    def getHealth(self):
        return self.health


hero1 = Hero("lina", 1350)
hero1.siapa() # namaku adalah lina
hero1.healthUp(200)
print(hero1.getHealth()) # 1550
```

# 5. Latihan OOP
```python
class Hero: #template
    def __init__(self, name, health): 
        self.name = name
        self.health = health
    
    # semua di python adalah object
    # disini lawan merupakan variabel untuk menampung object lain yang terbentuk dari class 
    def serang(self, lawan):
        print(self.name + " menyerang " + lawan.name)
        lawan.diserang(self)

    def diserang(self, lawan):
        print(self.name + " diserang " + lawan.name)

lina = Hero("Lina", 2000)
rikimaru = Hero("Rikimaru", 1200)

lina.serang(rikimaru)
# Lina menyerang Rikimaru
# Rikimaru diserang Lina
```

# 6. Tkinter
`pass`

# 7. Private Variable
```python
class Hero: #template
    def __init__(self, name, health): 
        # variaabel instance public 
        self.name = name
        self.health = health
        # variaabel instance private
        self.__variabelTakBisaDiubah = "private"
        # variabel instance protected
        self._variabelDalamClassAja = "protected"

lina = Hero("Lina", 2000)

print(lina.__dict__)
# {'name': 'Lina', 'health': 2000, '_Hero__variabelTakBisaDiubah': 'private', '_variabelDalamClassAja': 'protected'}
```
# 8. Encapsulation
- Enkapsulasi :
    - buat semua variabel private
    - untuk mengambil dan mensetting variable pakenya getter dan setter
```python
class Hero:
    def __init__(self, name, health, attackPower): 
        self.__name = name
        self.__health = health
        self.__attackPower = attackPower

    # getter
    def getName(self):
        return self.__name
    def getHealth(self):
        return self.__health

    #setter
    def diserang(self, val):
        self.__health -= val


# awal dari game
earthshaker = Hero("earthshaker", 1350, 57)

# game berjalan
print(earthshaker.getName()) # earthshaker
print(earthshaker.getHealth()) # 1350
earthshaker.diserang(200)
print(earthshaker.getHealth()) # 1150
```
# 9. Static dan Class Method
- cara biar static : pake decorator
```python
class Hero:
    __jumlah = 0

    def __init__(self, name): 
        self.__name = name
        Hero.__jumlah += 1

    # method ini hanya berlaku untuk object (nempel di object)
    def getJumlahByObject(self):
        return Hero.__jumlah

    # nempel di class tapi tidak nempel di object
    def getJumlah():
        return Hero.__jumlah

    # static method (pake decorator) nempel di object dan nempel di class nya
    @staticmethod
    def getJumlahByStatic():
        return Hero.__jumlah
    # nempel di object dan nempel di class nya, bedanya dia pake argumen
    @classmethod
    def getJumlahByClass(cls):
        return cls.__jumlah


sniper = Hero("sniper")
rikimaru = Hero("rikimaru")

print(sniper.getJumlahByObject()) # 2
print(Hero.getJumlah()) # 2
print(sniper.getJumlahByStatic()) # 2
print(Hero.getJumlahByStatic()) # 2
print(sniper.getJumlahByClass()) # 2
print(Hero.getJumlahByClass()) # 2
```
# 10. Mengubah method menjadi seperti property 
```python
class Hero:
    
    def __init__(self, name, health):
        self.__name = name
        self.__health = health

    @property # mengubah method menjadi seperti property
    def info(self):
        return "name : {}\nheath : {}".format(self.__name, self.__health)

    # 4 dibawah merupakan mengubah method menjadi seperti property untuk getter, setter dan deleter
    @property
    def health(self): # buat rujukan decorator
        pass

    @health.getter 
    def  health(self):
        return self.__health

    @health.setter
    def health(self, val):
        self.__health = val
    
    @health.deleter
    def health(self):
        self.__health = None


sniper = Hero("sniper", 1350)
print(sniper.info)
# name : sniper
# heath : 1350
print(sniper.health)
sniper.health = 20
print(sniper.health)
del sniper.health
print(sniper.health)
# name : sniper
# heath : 1350
# 1350
# 20
# None
```
# 11. Latihan Encapsulation
```python
class Hero:
    
    def __init__(self, name, health, attPower, armor):
        self.__name = name
        self.__healthStandar = health
        self.__attPower = attPower
        self.__armor = armor
        self.__level = 1
        self.__exp = 0
    
        # kenaikan level
        self.__healthMax = self.__healthStandar * self.__level
        self.__armor = self.__armor * self.__level
        self.__attPower = self.__attPower * self.__level

        self.__health = self.__healthMax

    @property
    def info(self):
        return "{} level {} : \n\thealth : {}/{}\n\tattack power : {}\n\tarmor : {}".format(self.__name, self.__level, self.__health, self.__healthMax, self.__attPower, self.__armor)
    
    @property
    def getExp(self):
        pass
    @getExp.setter
    def getExp(self, addExp):
        self.__exp += addExp
        if (self.__exp) >= 100 :
            print("{} level up".format(self.__name))
            self.__level += 1
            self.__exp -= 100

            # kenaikan level
            self.__healthMax = self.__healthStandar * self.__level
            self.__armor = self.__armor * self.__level
            self.__attPower = self.__attPower * self.__level


    
slardar = Hero("slardar", 100, 10, 5)
print(slardar.info)
slardar.getExp = 120
slardar.getExp = 120
print(slardar.info)
```

# 12. Pendahuluan Inheritance
```python
class Hero:

    def __init__(self, name, health):
        self.name = name
        self.health = health


# karena class ini inherit dari class Hero, class ini pya semua yang ada di class Hero
class HeroIntelegent(Hero):
    pass


lina = HeroIntelegent('lina', 100)
tinker = HeroIntelegent('tinker', 80)

print(lina.name)
print(tinker.name)
```
# 13. Super
- super : mengambil method/attr dari parent class untuk dipakai di subclass 
```python
class Hero:

    def __init__(self, name, health):
        self.name = name
        self.health = health


class HeroIntelegent(Hero):
    def __init__(self, name):
    # cara nggk efektif :
        # self.name = name
        # self.health = 100
    # cara pake nama parent class :
        # Hero.__init__(self, name, 100)
    # cara terefektif pake super() :
        super().__init__(name, 100)


lina = HeroIntelegent('lina')
print(lina.name, lina.health)
# lina 100
```

# 14. Override Method
```python
class Hero:

    def __init__(self, name, health):
        self.name = name
        self.health = health
    
    def info(self):
        print("{} health : {}".format(self.name, self.health))


class HeroIntelegent(Hero):
    def __init__(self, name):
        super().__init__(name, 100)

    # di override method infonya
    def info(self):
        print("{} health : {} hero intelegent".format(self.name, self.health))


lina = HeroIntelegent('lina')
lina.info()
# lina health : 100 hero intelegent
```

# 15. Latihan Inheritance
 pass

# 16. Multiple Inheritance
```python
class Team :
    def setTeam(self, team):
        self.team = team
    
    def showTeam(self):
        print(self.team)


class Type :
    def setType(self, type):
        self.type = type
    
    def showType(self):
        print(self.type)


class Hero(Team, Type):
    def __init__(self, name):
        self.name = name


lina = Hero("lina")
lina.setTeam("radiant")
lina.setType("range")
lina.showTeam()
lina.showType()
```

# 17. Method Resolution Order
- urutan Multiple Inheritance yang nama methodnya ada yang sama : uratannya dari yang disebut duluan
```python
class A:
    def show(self):
        print("ini show A")

class B:
    def show(self):
        print("ini show B")

class C(A,B):
    pass

object = C()
object.show() # ini adalah show A
help(object) # buat liat urutan
# Help on C in module __main__ object:

# class C(A, B)
#  |  Method resolution order:
#  |      C
#  |      A
#  |      B
#  |      builtins.object
#  |  
#  |  Methods inherited from A:
#  |  
#  |  show(self)
#  |  
#  |  ----------------------------------------------------------------------
#  |  Data descriptors inherited from A:
#  |  
#  |  __dict__
#  |      dictionary for instance variables (if defined)
#  |  
#  |  __weakref__
#  |      list of weak references to the object (if defined)
# (END)
```

# 18. Diamond Problem
- Masalah yang muncul dari multiple inheritance jika :

    ![](python-oop/diamond_problem.png?raw=true)
- urutannya di python adalah dari D ke B ke C baru ke A
```python
class A:
    def show(self):
        print("ini show A")


class B:
    def show(self):
        print("ini show B")


class C:
    def show(self):
        print("ini show C")


class D(B,C):
    pass


object = D()
object.show() # ini adalah show B
help(object)
# Help on D in module __main__ object:

# class D(B, C)
#  |  Method resolution order:
#  |      D
#  |      B
#  |      C
#  |      builtins.object
#  |  
#  |  Methods inherited from B:
#  |  
#  |  show(self)
#  |  
#  |  ----------------------------------------------------------------------
#  |  Data descriptors inherited from B:
#  |  
#  |  __dict__
#  |      dictionary for instance variables (if defined)
#  |  
#  |  __weakref__
#  |      list of weak references to the object (if defined)
# (END)
```

# 19. Magic Method
```python
class Hero:

    # tanda magic method : __namamethod__

    # fungsi yang sekali dan pertama kali dipanggil ketika objet dibuat
    def __init__(self, name, health):
        self.name = name
        self.health = health

    # ketika object di print apa yang mau ditampilin
    def __str__(self):
        return ("Hero : {}".format(self.name))

    # mirip __str__ tapi biasanya buat debugging
    def __repr__(self):
        return ("Debug : Hero : {}".format(self.name))

    # buat tambahin 2 object 
    def __add__(self, obj):
        return self.health + obj.health
    

lina = Hero("lina", 2300)
sniper = Hero("sniper", 1200)

print(lina) # Hero : lina
print(repr(lina)) # Debug : Hero : lina
print (lina + sniper)  # 3500
```

# 20. Class Abstract
```python
# kenapa harus import? karena python nggk punya syntax buat jadiinya class abstract
from abc import ABC, abstractmethod # abstract base class

class Button(ABC):   

    @abstractmethod
    def click(self):
        pass


class PushButton(Button):
    def click(self):
        print("Push buttton di click")


tombol = PushButton()
tombol.click()
```

# 21. Class Abstract dan Decorator

