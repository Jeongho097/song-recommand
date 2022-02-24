# song-recommand

# 1. 불용어 데이터 크롤링
  - 다른 유저들이 한국어 불용어를 정리해 놓은 데이터를 크롤링함.

# 2. mecab을 통한 전처리
  - 데이터는 카카오 아레나의 Melon Playlist Continuation 데이터를 사용

# 3_1. word2vec를 통한 word embedding
  - word embedding을 진행한 후 hnswlib knn을 유사도 계산

# 3_2. sent2vec를 통한 word embedding
  - word embedding을 진행한 후 hnswlib knn을 유사도 계산

# 4_1. word2vec 기반 노래 추천
  - 추천을 받고 싶은 상황이나 가수 등을 문장으로 입력하면 해당 문장과 가장 유사하다고 생각되는 노래들을 word2vec 기반으로 노래를 추천한다

# 4_2. sent22vec 기반 노래 추천
  - 추천을 받고 싶은 상황이나 가수 등을 문장으로 입력하면 해당 문장과 가장 유사하다고 생각되는 노래들을 sent2vec 기반으로 노래를 추천한다

# 5. wordcloud
 - 일반화 할 수 있는 점수가 존재하지 않기 때문에 입력한 문장과 관련된 태그들을 wordlcloud를 통해 보여줌
 - 입력한 문장이 짧다면 word2vec와 sent2vec의 차이가 크게 느껴지지 않는다 
 - 하지만 입력한 문장의 길이가 길어진다면 문장의 의미를 파악해 해당 문장과 관련된 다양한 단어를 많이 보여준다
 - -따라서 원하는 노래를 추천받기 위해서 word2vec보다는 sent2vec의 사용이 필요하다고 느끼고 sent2vec을 최종 모델로 선택함
