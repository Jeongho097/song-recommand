# song-recommand

* 비즈니스 인사이트 도출을 위한 데이터 분석 프로젝트 과정(메디치교육센터)
  - 최종 프로젝트로 진행한 추천시스템

# 
카카오 아레나 대회의 멜론 플레이리스트 데이터, song_meta 데이터를 이용해 추천 시스템을 구축함.
 - https://arena.kakao.com/c/8/data

1. 플레이리스트의 태그와 플레이 리스트 타이틀을 자연어 전처리를 통해 단어의 제한과 통일을 함.
- 잔잔한, 잔잔하게, 잔잔함 또는 sg 워너비, sg 워너비, SG 워너비 등 단어의 의미는 같으나 단어의 형태가 다르기 때문에 후에 유사도를 구할 때 문제가 있을 것이라 판단하여 위의 예시들을 konlpy의 mecab을 통해 잔잔 / sg 워너비 이런 식으로 제한하고 통일함.
- '센치한' 과같이 국립국어원에 등록되어 있지 않은 신조어 또는 기타 단어들은 사용자 사전을 통해서 새로 추가해줌
- 단어 비용이 기존 단어 사전의 단어 비용보다 높아 출력되지 않는 경우(예시 : '소녀시대' >> '소녀' '시대') 단어 비용을 낮춤으로써 문제를 해결
- 위와 같은 과정을 거친 후 고유명사, 일반명사, 형용사, 동사, 외국어, 숫자, 어근에 해당하는 품사만 추출해 new_tag라는 파생 변수로 합해줌

2. 사용자의 의도를 반영한 노래 추천
- 자연어 전처리를 통해 나온 new_tag는 제한과 통일을 통해 나온 단어들이기 때문에 모든 단어들을 의미 있는 단어로 판단하고 모든 단어를 사용하기로 결정하지만 word2vec는 각각의 단어에 대한 유의미한 결과를 도출해낼 수 있지만 이것을 문장 단위 이상에서의 유의미한 결과를 도출해 내는 것은 쉽지 않기 때문에 문장의 의미를 살릴 수 있는 Sent2Vec을 사용함.
- 여기서 사용된 Sent2Vec은 word2vec의 CBOW를 확장함. 또한 fastText 라이브러리에 알고리즘을 추가해 개선하는 형식으로 사용되기 때문에 문장 단위뿐만 아니라 단어 단위에서도 유의미한 의미를 찾을 수가 있음.
- 여기서 중요한 것은 사용자는 본인이 원하는 노래와 관련된 단어/문장을 입력해 노래를 추천받을 수 있다.
상황, 분위기, 가수, 장르 (예시 : 카페에서 공부하면서 듣기 좋은 노래 / sg워너비 노래 / 빅뱅, 2PM, 소녀시대 / 디즈니 영화 ost 등) 다양한 입력을 받고 입력받은 값과 문장을 가진 플레이리스트를 찾아내 해당 플레이리스트에서 노래를 추천받을 수 있다.

3. HNSW - Fast Approximate Nearest Neighbor Search
- 추천 시스템이나 NLP에서는 Vector의 최근접을 찾는 방식이 필요
- sklearn의 KNN의 경우 높은 정확도를 보이나 데이터 크기에 비례해 많은 시간이 소요
- 따라서 시간이 적게 들고 KNN과 같이 높은 정확도를 보이는 ANN알고리즘 HNSW사용
![속도 비교(참고 : https://ichi.pro/ko/knn-k-nearest-neighbors-i-jug-eossseubnida-17323298122558)](https://ichi.pro/assets/images/max/724/1*Y879CpkO8S6L-vo_PbqutA.png)
![정확도(참고 : https://ichi.pro/ko/knn-k-nearest-neighbors-i-jug-eossseubnida-17323298122558)](https://ichi.pro/assets/images/max/724/1*5ZBt0ITNNOXD-HD4Rxs0bg.png)
![속도 비교(참고 : https://ichi.pro/ko/knn-k-nearest-neighbors-i-jug-eossseubnida-17323298122558)]<img width="531" alt="KakaoTalk_20220321_165055392" src="https://user-images.githubusercontent.com/89580953/159222009-b4777f5c-5876-4644-943e-dec3201c78ec.png">



# 1. 불용어 데이터 크롤링
  - 다른 유저들이 한국어 불용어를 정리해 놓은 데이터를 크롤링함.

# 2. Mecab을 통한 전처리
  - 데이터는 카카오 아레나의 Melon Playlist Continuation 데이터를 사용

# 3_1. Word2Vec를 통한 word embedding
  - word embedding을 진행한 후 hnswlib knn를 통해 유사도 계산

# 3_2. Sent2Vec를 통한 word embedding
  - word embedding을 진행한 후 hnswlib knn를 통해 유사도 계산
  - sent2vec의 경우 ubuntu 서버를 이용해서 편하게 다운 사용이 가능함

# 4_1. Word2Vec 기반 노래 추천
  - 추천을 받고 싶은 상황이나 가수 등을 문장으로 입력하면 해당 문장과 가장 유사하다고 생각되는 노래들을 word2vec 기반으로 노래를 추천한다

# 4_2. Sent2Vec 기반 노래 추천
  - 추천을 받고 싶은 상황이나 가수 등을 문장으로 입력하면 해당 문장과 가장 유사한 노래들을 sent2vec 기반으로 노래를 추천한다

# 5. WordCloud
 - 일반화 할 수 있는 점수가 존재하지 않기 때문에 입력한 문장과 관련된 태그들을 wordlcloud를 통해 보여줌
 - 입력한 문장이 짧다면 word2vec와 sent2vec의 차이가 크게 느껴지지 않는다 
 - 하지만 입력한 문장의 길이가 길어진다면 문장의 의미를 파악해 해당 문장과 관련된 다양한 단어를 많이 보여준다
 - -따라서 원하는 노래를 추천받기 위해서 word2vec보다는 sent2vec의 사용이 필요하다고 느끼고 sent2vec을 최종 모델로 선택함

# 6. 
프로젝트 당시에는 아마존 EC2를 이용해 홈페이지를 생성함 (현재는 존재하지 않음)
![image](https://user-images.githubusercontent.com/89580953/155469543-bb18256b-2b58-47c8-861b-4e4b41818fa1.png)

위와 같이 유저의 데이터가 존재한다면 유저가 생성한 플레이리스트을 기반으로 추천 태그들을 볼 수 있고 입력을 통해 노래를 추천받을 수 있음
 
