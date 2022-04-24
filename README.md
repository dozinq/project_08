# 📌 My Eighth Project 📋

---

##### - Outline : 2022년 4월 22일, 여덟번째 관통 프로젝트를 수행하였다. 오늘 프로젝트를 하면서 느낀 점부터 한 줄로 간단하게 말하자면, '내가 피해가 되지 않으려면 평소에 남들보다 더 열심히 해야 한다.' 였다. 사실 오늘 수행하면서 이번 주에 배웠던 DRF를 응용하려다 보니 복습이 제대로 되지 않았던 나는 헤맬 수 밖에 없었다. 매우 처참했지만, 내 자신이 불쌍하진 않았다. 그저 그 전에 노력을 안했기 때문에 얻을 수 있는 당연한 결과였다. 현실을 받아들이되, 앞으로 더 열심히 공부를 해야겠다는 생각이 프로젝트 중에 계속 들었다. 그리고 이 README를 작성하고 있는 지금, 사실 몇몇의 기능이 제대로 수행되지 않는다. 이 점은 반드시 주말에 고칠 수 있도록 할 것이다. 오늘은 같은 반의 '백경원' 학우와 페어를 이루어 프로젝트를 수행하였고, 그 과정 속에서 느낀 점을 중심으로 서술해 보고자 한다. 또한 개선해야 하는 부분도 작성해보려 한다.

---

<br>


# **< Title : "DB 설계를 활용한 REST API 설계" >**

*(This project was carried out in **Python 3.9.9 and Django 3.2.12 environment .**)*

- *요구사항 : 모델 간의 관계 설정 후 Actor를 통해 배우 데이터를 조회하고, Movie를 통해 영화 데이터를 조회하고, Review를 통해 리뷰 데이터를 조회, 생성, 수정, 삭제를 할 수 있는 기능을 구현시켜야 한다.*

---

*(프로젝트 파일의 settings.py, urls.py와 base.html 등의 기본 설정은 제외하고 , 앱 파일에 관해서만 작성합니다.)*

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

  : 명세서에 적혀있는 대로 url이 입력된다면 알맞은 view 함수에 갈 수 있도록 해주었다. 워낙 많이 해 본 작업이다보니 많이 수월하게 진행할 수 있었던 것 같다.

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
              'delete': f'{review_pk}번 리뷰가 삭제되었습니다.',
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

  : 사실 이번 주에 배운 내용을 그대로 활용할 수 있었던 view 함수 구현이었으며, 이 부분과 이후에 작성할 serializers.py 부분으로 많이 고민하였던 것 같다. 그래도 이전 수업 내용을 참고하며 수월하게 작성할 수 있었으며, 각 요구사항에 맞게 출력할 수 있어야 했는데, 그 부분에 대해서는 이후에 많은 학습이 필요할 것이라고 판단되었다.

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

  : 주어진 조건에 맞게 models.py를 구성해 주었다. 이 부분 또한 명세서에 나와있는 대로 진행하다 보니 수월하였는데, ManyToManyField를 구현하는 것에 있어서 많은 생각을 해야만 하였다. 더 자연스럽게 작성할 수 있도록 노력할 것이다.

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

  : 이번 관통 프로젝트의 핵심 부분이었던 serializer의 구현이었다. 수업을 들을 때는 꽤나 수월하였던 것 같은데, 많이 힘들었던 것이 사실이다. 또한 이 부분을 앞으로 잘 개선해 준다면 원하는 내용만을 출력하고 Client에게 응답시킬 수 있다는 것을 느꼈고, 중요성을 깨달았다. 이번 주말 동안 많이 시도해보며 능력을 쌓아가려 한다.

<br>

---

## ✏After finishing..

>   　　　　　벌써 여덟번째 관통 프로젝트를 진행하였다. 배우는 내용이 정말 많은 것도 사실이지만, 그것을 내 것으로 제대로 만드려고 노력하는 데도 그 노력이 부족한 것도 사실이다. 최종 프로젝트도 얼마 남지 않았는데, 많이 개선해 나아가고자 한다. 앞으로 누군가에게 도움이 되고만 싶다.
>
>   　　　　​	오늘 프로젝트가 끝나자마자 교수님께서 더 이상 수업을 못한다고 하셨다. 같은 반 학우들도 나와 같이 많이 슬퍼하였고, 아쉬운 마음이 아직도 가시질 않는다. 수업이 끝나고 README파일을 작성해야 했는데, 아무것도 하기 싫다는 기분을 처음 느껴본 것 같다. 미루고 미루다가 자정이 되기 전에 급하게 작성하긴 하였는데, 정말 엉망이다. 앞으로 씩씩하게 내 할 일 하다보면 언젠간 만날 수 있을거라 생각한다. 내가 당장 해야 할 일에 집중하다가 멀지 않은 시간이 지나고 꼭 좋은 모습으로 마주하고 싶다.
>
>   　　　　​	JUSTIN 너무 감사했습니다. 같이 시간을 보낼 수 있었던 것은, 제 인생에 몇 없을 행운 중 하나일 것입니다. 어디에 계셔도 항상 건강하셨으면 좋겠습니다.