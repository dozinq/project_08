# π My Eighth Project π

---

##### - Outline : 2022λ 4μ 22μΌ, μ¬λλ²μ§Έ κ΄ν΅ νλ‘μ νΈλ₯Ό μννμλ€. μ€λ νλ‘μ νΈλ₯Ό νλ©΄μ λλ μ λΆν° ν μ€λ‘ κ°λ¨νκ² λ§νμλ©΄, 'λ΄κ° νΌν΄κ° λμ§ μμΌλ €λ©΄ νμμ λ¨λ€λ³΄λ€ λ μ΄μ¬ν ν΄μΌ νλ€.' μλ€. μ¬μ€ μ€λ μννλ©΄μ μ΄λ² μ£Όμ λ°°μ λ DRFλ₯Ό μμ©νλ €λ€ λ³΄λ λ³΅μ΅μ΄ μ λλ‘ λμ§ μμλ λλ ν€λ§¬ μ λ°μ μμλ€. λ§€μ° μ²μ°Ένμ§λ§, λ΄ μμ μ΄ λΆμνμ§ μμλ€. κ·Έμ  κ·Έ μ μ λΈλ ₯μ μνκΈ° λλ¬Έμ μ»μ μ μλ λΉμ°ν κ²°κ³Όμλ€. νμ€μ λ°μλ€μ΄λ, μμΌλ‘ λ μ΄μ¬ν κ³΅λΆλ₯Ό ν΄μΌκ² λ€λ μκ°μ΄ νλ‘μ νΈ μ€μ κ³μ λ€μλ€. κ·Έλ¦¬κ³  μ΄ READMEλ₯Ό μμ±νκ³  μλ μ§κΈ, μ¬μ€ λͺλͺμ κΈ°λ₯μ΄ μ λλ‘ μνλμ§ μλλ€. μ΄ μ μ λ°λμ μ£Όλ§μ κ³ μΉ  μ μλλ‘ ν  κ²μ΄λ€. μ€λμ κ°μ λ°μ 'λ°±κ²½μ' νμ°μ νμ΄λ₯Ό μ΄λ£¨μ΄ νλ‘μ νΈλ₯Ό μννμκ³ , κ·Έ κ³Όμ  μμμ λλ μ μ μ€μ¬μΌλ‘ μμ ν΄ λ³΄κ³ μ νλ€. λν κ°μ ν΄μΌ νλ λΆλΆλ μμ±ν΄λ³΄λ € νλ€.

---

<br>


# **< Title : "DB μ€κ³λ₯Ό νμ©ν REST API μ€κ³" >**

*(This project was carried out in **Python 3.9.9 and Django 3.2.12 environment .**)*

- *μκ΅¬μ¬ν­ : λͺ¨λΈ κ°μ κ΄κ³ μ€μ  ν Actorλ₯Ό ν΅ν΄ λ°°μ° λ°μ΄ν°λ₯Ό μ‘°ννκ³ , Movieλ₯Ό ν΅ν΄ μν λ°μ΄ν°λ₯Ό μ‘°ννκ³ , Reviewλ₯Ό ν΅ν΄ λ¦¬λ·° λ°μ΄ν°λ₯Ό μ‘°ν, μμ±, μμ , μ­μ λ₯Ό ν  μ μλ κΈ°λ₯μ κ΅¬νμμΌμΌ νλ€.*

---

*(νλ‘μ νΈ νμΌμ settings.py, urls.pyμ base.html λ±μ κΈ°λ³Έ μ€μ μ μ μΈνκ³  , μ± νμΌμ κ΄ν΄μλ§ μμ±ν©λλ€.)*

<br>

- **movies/urls.py**

  ```python
  from django.urls import path
  from . import views
  
  urlpatterns = [
      path('actors/', views.actor_list),
      path('actors/<int:actor_pk>/', views.actor_detail),
      path('movies/', views.movie_list),
      path('movies/<int:movie_pk>/', views.movie_detail),
      path('movies/<int:movie_pk>/reviews/', views.create_review),
      path('reviews/', views.review_list),
      path('reviews/<int:review_pk>/', views.review_detail),
  ]
  ```

  : λͺμΈμμ μ νμλ λλ‘ urlμ΄ μλ ₯λλ€λ©΄ μλ§μ view ν¨μμ κ° μ μλλ‘ ν΄μ£Όμλ€. μλ λ§μ΄ ν΄ λ³Έ μμμ΄λ€λ³΄λ λ§μ΄ μμνκ² μ§νν  μ μμλ κ² κ°λ€.

<br>

- **movies/views.py**

  ```python
  from rest_framework.decorators import api_view
  from rest_framework import status
  from rest_framework.response import Response
  
  from django.shortcuts import get_list_or_404, get_object_or_404
  
  from movies.serializers import(
      ActorListSerializer, MovieListSerializer, ReviewListSerializer,
      MovieSerializer, ReviewSerializer, ActorSerializer
  )
  from .models import Actor, Movie, Review
  
  
  @api_view(['GET'])
  def actor_list(request):
      actors = get_list_or_404(Actor)
      serializer = ActorListSerializer(actors, many=True)
      return Response(serializer.data)
  
  @api_view(['GET'])
  def actor_detail(request, actor_pk):
      actor = get_object_or_404(Actor, pk=actor_pk)
      serializer = ActorSerializer(actor)
      return Response(serializer.data)
  
  @api_view(['GET'])
  def movie_list(request):
      movies = get_list_or_404(Movie)
      serializer = MovieListSerializer(movies, many=True)
      return Response(serializer.data)
  
  @api_view(['GET'])
  def movie_detail(request, movie_pk):
      movie = get_object_or_404(Actor, pk=movie_pk)
      serializer = MovieSerializer(movie)
      return Response(serializer.data)
  
  @api_view(['GET'])
  def review_list(request):
      reviews = get_list_or_404(Review)
      serializer = ReviewListSerializer(reviews, many=True)
      return Response(serializer.data)
  
  @api_view(['GET', 'PUT', 'DELETE'])
  def review_detail(request, review_pk):
      review = get_object_or_404(Review, pk=review_pk)
  
      if request.method == 'GET':
          serializer = ReviewSerializer(review)
          return Response(serializer.data)
      if request.method == 'PUT':
          serializer = ReviewSerializer(review, request.data)
          if serializer.is_valid(raise_exception=True):
              serializer.save()
              return Response(serializer.data)
      if request.method == 'DELETE':
          review.delete()
          data = {
              'delete': f'{review_pk}λ² λ¦¬λ·°κ° μ­μ λμμ΅λλ€.',
          }
          return Response(data, status=status.HTTP_204_NO_CONTENT)
  
  @api_view(['POST'])
  def create_review(request, movie_pk):
      movie = get_object_or_404(Movie, pk=movie_pk)
      serializer = ReviewSerializer(data=request.data)
      if serializer.is_valid(raise_exception=True):
          serializer.save(movie=movie)
          return Response(serializer.data, status=status.HTTP_201_CREATED)
  ```

  : μ¬μ€ μ΄λ² μ£Όμ λ°°μ΄ λ΄μ©μ κ·Έλλ‘ νμ©ν  μ μμλ view ν¨μ κ΅¬νμ΄μμΌλ©°, μ΄ λΆλΆκ³Ό μ΄νμ μμ±ν  serializers.py λΆλΆμΌλ‘ λ§μ΄ κ³ λ―Όνμλ κ² κ°λ€. κ·Έλλ μ΄μ  μμ λ΄μ©μ μ°Έκ³ νλ©° μμνκ² μμ±ν  μ μμμΌλ©°, κ° μκ΅¬μ¬ν­μ λ§κ² μΆλ ₯ν  μ μμ΄μΌ νλλ°, κ·Έ λΆλΆμ λν΄μλ μ΄νμ λ§μ νμ΅μ΄ νμν  κ²μ΄λΌκ³  νλ¨λμλ€.

<br>

- **movies/models.py**

  ```python
  from django.db import models
  
  class Actor(models.Model):
      name = models.CharField(max_length=100)
  
  class Movie(models.Model):
      title = models.CharField(max_length=100)
      overview = models.TextField()
      release_date = models.DateTimeField()
      poster_path = models.TextField()
      actors = models.ManyToManyField(Actor, related_name='movies_actor')
  
  class Review(models.Model):
      title = models.CharField(max_length=100)
      content = models.TextField()
      movie = models.ForeignKey(Movie, on_delete=models.CASCADE)
  ```

  : μ£Όμ΄μ§ μ‘°κ±΄μ λ§κ² models.pyλ₯Ό κ΅¬μ±ν΄ μ£Όμλ€. μ΄ λΆλΆ λν λͺμΈμμ λμμλ λλ‘ μ§ννλ€ λ³΄λ μμνμλλ°, ManyToManyFieldλ₯Ό κ΅¬ννλ κ²μ μμ΄μ λ§μ μκ°μ ν΄μΌλ§ νμλ€. λ μμ°μ€λ½κ² μμ±ν  μ μλλ‘ λΈλ ₯ν  κ²μ΄λ€.

- **movies/serializers.py**

  ```python
  from asyncore import read
  from rest_framework import serializers
  from .models import Actor, Movie, Review
  
  class ActorListSerializer(serializers.ModelSerializer):
  
      class Meta:
          model = Actor
          fields = '__all__'
  
  class MovieListSerializer(serializers.ModelSerializer):
  
      class Meta:
          model = Movie
          fields = ('title', 'overview',)
  
  class ReviewListSerializer(serializers.ModelSerializer):
  
      class Meta:
          model = Review
          fields = ('title', 'content',)
  
  class ReviewSerializer(serializers.ModelSerializer):
  
      class Meta:
          model = Review
          fields = ('id', 'title', 'content',)
  
  class MovieSerializer(serializers.ModelSerializer):
      class ActorSerializer(serializers.ModelSerializer):
  
          class Meta:
              model = Actor
              fields = ('title',)
  
      actors = ActorSerializer(many=True, read_only=True)
      review_set = ReviewListSerializer(many=True, read_only=True)
  
      class Meta:
          model = Movie
          fields = '__all__'
  ```

  : μ΄λ² κ΄ν΅ νλ‘μ νΈμ ν΅μ¬ λΆλΆμ΄μλ serializerμ κ΅¬νμ΄μλ€. μμμ λ€μ λλ κ½€λ μμνμλ κ² κ°μλ°, λ§μ΄ νλ€μλ κ²μ΄ μ¬μ€μ΄λ€. λν μ΄ λΆλΆμ μμΌλ‘ μ κ°μ ν΄ μ€λ€λ©΄ μνλ λ΄μ©λ§μ μΆλ ₯νκ³  Clientμκ² μλ΅μν¬ μ μλ€λ κ²μ λκΌκ³ , μ€μμ±μ κΉ¨λ¬μλ€. μ΄λ² μ£Όλ§ λμ λ§μ΄ μλν΄λ³΄λ©° λ₯λ ₯μ μμκ°λ € νλ€.

<br>

---

## βAfter finishing..

>   γγγγγλ²μ¨ μ¬λλ²μ§Έ κ΄ν΅ νλ‘μ νΈλ₯Ό μ§ννμλ€. λ°°μ°λ λ΄μ©μ΄ μ λ§ λ§μ κ²λ μ¬μ€μ΄μ§λ§, κ·Έκ²μ λ΄ κ²μΌλ‘ μ λλ‘ λ§λλ €κ³  λΈλ ₯νλ λ°λ κ·Έ λΈλ ₯μ΄ λΆμ‘±ν κ²λ μ¬μ€μ΄λ€. μ΅μ’ νλ‘μ νΈλ μΌλ§ λ¨μ§ μμλλ°, λ§μ΄ κ°μ ν΄ λμκ°κ³ μ νλ€. μμΌλ‘ λκ΅°κ°μκ² λμμ΄ λκ³ λ§ μΆλ€.
>
>   γγγγβ	μ€λ νλ‘μ νΈκ° λλμλ§μ κ΅μλκ»μ λ μ΄μ μμμ λͺ»νλ€κ³  νμ¨λ€. κ°μ λ° νμ°λ€λ λμ κ°μ΄ λ§μ΄ μ¬νΌνμκ³ , μμ¬μ΄ λ§μμ΄ μμ§λ κ°μμ§ μλλ€. μμμ΄ λλκ³  READMEνμΌμ μμ±ν΄μΌ νλλ°, μλ¬΄κ²λ νκΈ° μ«λ€λ κΈ°λΆμ μ²μ λκ»΄λ³Έ κ² κ°λ€. λ―Έλ£¨κ³  λ―Έλ£¨λ€κ° μμ μ΄ λκΈ° μ μ κΈνκ² μμ±νκΈ΄ νμλλ°, μ λ§ μλ§μ΄λ€. μμΌλ‘ μ©μ©νκ² λ΄ ν  μΌ νλ€λ³΄λ©΄ μΈμ  κ° λ§λ  μ μμκ±°λΌ μκ°νλ€. λ΄κ° λΉμ₯ ν΄μΌ ν  μΌμ μ§μ€νλ€κ° λ©μ§ μμ μκ°μ΄ μ§λκ³  κΌ­ μ’μ λͺ¨μ΅μΌλ‘ λ§μ£Όνκ³  μΆλ€.
>
>   γγγγβ	JUSTIN λλ¬΄ κ°μ¬νμ΅λλ€. κ°μ΄ μκ°μ λ³΄λΌ μ μμλ κ²μ, μ  μΈμμ λͺ μμ νμ΄ μ€ νλμΌ κ²μλλ€. μ΄λμ κ³μλ ν­μ κ±΄κ°νμ¨μΌλ©΄ μ’κ² μ΅λλ€.