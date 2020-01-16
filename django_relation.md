## 1. One to Many
```python
class Language(models.Model):
    name = models.CharField(max_length=10)

    def __str__(self):
        return self.name

class Framework(models.Model):
    name = models.CharField(max_length=10)
    language = models.ForeignKey(Language, on_delete=models.CASCADE)

    def __str__(self):
        return self.name
```
## 2. Many to Many
- u can put in either table but u want to put it on the one that conceptually a lower level, so a movie that is on higher level than character 
- so put it on character that the way lower level
    ```python
    class Movie(models.Model):
        name = models.CharField(max_length=10)


        def __str__(self):
            return self.name

    class Character(models.Model):
        name = models.CharField(max_length=10)
        movies = models.ManyToManyField(Movie)

        def __str__(self):
            return self.name
    ```
## 3. One to One
- Contoh : Restoran dan Alamat, 1 Restoran hanya memiiki 1 Alamat
- Taro field one to one di yang lebih rendah derajatnya.

    ```python
    class Place(models.Model):
        name = models.CharField(max_length=50)
        address = models.CharField(max_length=80)

        def __str__(self):
            return self.name

    class Restaurant(models.Model):
        place = models.OneToOneField(
            Place,
            on_delete=models.CASCADE,
            primary_key=True,
        )
        serves_hot_dogs = models.BooleanField(default=False)
        serves_pizza = models.BooleanField(default=False)

        def __str__(self):
            return self.place.name
    ```