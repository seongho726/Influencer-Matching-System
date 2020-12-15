# 문제

- **Q1 : View와 다중 join을 활용한 문제**

    무료회원 a는 기본적으로 제공되는 인플루언서들의 정보(name, target, category, channel, image)가 너무 적어 더 많은 정보를 위해 유료결제를 한다. 이에 따른 새로운 정보(year ,target_cha, cost, youtube_id, insta_id, blog_id)가 추가된 table을 만들어라

    **[답안 도출 과정]**

    - 기본 제공되는 정보 table, 추가해야 할 테이블의 컬럼 확인

    ```sql
    select * from free
    desc youtube
    desc instagram
    desc blog
    ```

    - LEFT OUTER JOIN을 사용 해 여러 테이블의 컬럼값을 한번에 검색

    ```sql
    select i.name, target, year, category, target_cha, channel, cost, image, insta_id,
     youtube_id, blog_id
            from influencer i
            left outer join instagram g on (i.name=g.name)
            left outer join youtube y on (i.name=y.name)
            left outer join blog b on (i.name=b.name);
    ```

    - 최종 검색한 값들은 한 번에 묶을 가상의 VIEW를 만들어 사용자에게 제공

    ```sql
    create view membership
        as
            select i.name, target, year, category, target_cha, channel, cost, image, insta_id, youtube_id, blog_id
            from influencer i
            left outer join instagram g on (i.name=g.name)
            left outer join youtube y on (i.name=y.name)
            left outer join blog b on (i.name=b.name)
    ```

  ![img](https://i.imgur.com/g7fK27h.png)

- **Q2 : rownum조건을 활용해 순위 추출하기**

    회사 '늘푸른'의 패션 MD로부터 연락이 왔다. 이번 겨울 상품에 새로 디자인한 부츠를 인플루언서의 채널을 
    활용해 마케팅하려고 싶다고 한다. '유튜브'나 '인스타그램'을 사용할 예정이고 이미지는 '트렌디'하거나 '호감형'이길 원했다. 가격대는 150만원 이하로 순위를 평가할 수 있는 영향력지수도 함께 전달해달라고 했다. 
    해당되는 인플루언서 리스트 중 영향력 지수가 높은 사람 3순위를 뽑아서 해당 MD에게 1시간 내에 답을 주자. 
    리스트에는 해당 rownum, 인플루언서의 이름(name)과 카테고리(category), 이미지(image), 영향력점수(influence),  대표채널(channel), 비용(cost)에 대한 내용이 포함되어 있다.

    **[답안 도출 과정]**

    - **'**유튜브'나 '인스타그램'을 사용하면서 이미지는 '트렌디'하거나 '호감형'인 인플루언서를 먼저 찾아본다.

    ```sql
    select influencer.name, category, image, influence, channel, cost from influencer 
    where channel in ('유튜브', '인스타그램') and image in ('트렌디', '호감형');
    ```

    - 위의 조건을 만족하면서 가격대는 150만원 이하인 인플루언서를 검색한다.

    ```sql
    select influencer.name, category, image, influence, channel, cost from influencer 
    where cost < = 150 and channel in ('유튜브', '인스타그램') and image in ('트렌디', '호감형');
    ```

    - 최종 answer : rownum 조건 포함해 검색

    ```sql
    select rownum, name, category, image, influence, channel, cost
    from (select * from  influencer where channel in ('유튜브', '인스타그램') and image in ('트렌디', '호감형') and cost<= 150
    order by influence desc) where rownum <=3;
    ```
    ![img](https://i.imgur.com/5SqMPR2.png)

  

- **Q3 : inline subquery와 2중 조인을 활용한 문제[답안 도출 과정]**

    A회사는 인스타그램을 대표 채널로 하면서 유튜브와 블로그도 하는 인플루언서를 모델로 고용하고 싶어한다. 
    현재 A회사는 인스타그램 광고비용(Insta_cost)을 80 - 120만원을 생각하고 있다. 이 상황에서 가장 적합한 인플루언서를 검색하세요.   

    참고로 Influencer 테이블에 channel은 인플루언서별 대표채널을 의미한다.

    **[답안 도출 과정]**

    - 인스타그램을 대표채널로 하는 모델 검색

    ```sql
    select name, channel from influencer where channel = '인스타그램';
    ```

    - 인스타그램이 대표채널인 모델 중 광고단가가 80이상 120만원 이하인 모델 검색하기

    ```sql
    select i.name , insta_cost, youtube_id
    from instagram i, youtube y
    where insta_cost between 80 and 120 and i.name=y.name;
    ```

    - 최종 답안 : 인스타그램과 유튜브를 함께 운영하고 인스타그램 광고단가가 80이상 120이하인 모델의 유튜브 id 검색하기

    ```sql
    select name, youtube_id from(
            select i.name, y.youtube_id
            from instagram i, youtube y
            where insta_cost between 80 and 120 and i.name = y.name 
    );
    ```
    ![img](https://i.imgur.com/Yn5u7sF.png)
