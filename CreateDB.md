# 데이터베이스 생성
**ERD 구조를 위한 초기 기획 단계**
![img](https://i.imgur.com/5jUtT34.png)
**ERD구조 [[ERDCLOUD]](https://www.erdcloud.com/d/uzPPQZ5T2vH365Hto)**
![img](https://i.imgur.com/eX2uals.png)

**실제 TABLE 작성  [[google spreadsheets]](https://docs.google.com/spreadsheets/d/1EvV_pDHktw6e2mowog0opyU-CZJLH9zarZAGSOuPS74/edit?usp=sharing)**
![img](https://i.imgur.com/xS3cQ4I.png[/img])
**TABLE 생성 SQL구문**

- INFLUENCER

    ```sql
    --influencer table 생성
    create table influencer(
            name varchar2(20) constraint influencer_name_pk primary key, 
            target varchar2(10),
            year varchar2 (10) not null,
            Influence number (5),
            category varchar2(30) not null,
            target_cha varchar2(10),
            channel varchar2(10),
            cost number(20),
            image varchar2(10) 
    );

    -- 컬럼의 varchar 값 수정
    alter table influencer modify(image varchar2(12));
    alter table influencer modify(channel varchar2(20));

    -- influencer  table db 입력
    insert into influencer values('김연지', '30대', '2년', 2,'식품', '직장인', '유튜브', 100, '호감형');
    insert into influencer values('박다영', '20대', '1년', 2,'펫', '학생', '유튜브', 50, '호감형');
    insert into influencer values('최지수', '20대', '3년', 4,'뷰티', '여성', '유튜브', 200, '트렌디');
    insert into influencer values('이민재', '20대', '1개월', 1,'스포츠/레저/자동차', '외국인', '유튜브', 30, '코믹');
    insert into influencer values('장종욱', '30대', '6개월', 1,'뷰티', '남성', '유튜브', 300, '트렌디');
    insert into influencer values('박민영', '20대', '9개월', 1,'식품', '주부', '유튜브', 100, '건강');
    insert into influencer values('김혜경', '40대', '10년',5 ,'출산/육아', '주부', '유튜브', 1000, '트렌디');
    insert into influencer values('차왕현', '10대', '2년', 3,'펫', '영유아', '유튜브', 100, '호감형');
    insert into influencer values('양호준', '전연령', '5년', 5,'스포츠/레저/자동차', '남성', '유튜브', 500, '건강');
    insert into influencer values('권희성', '20대', '3년', 3, '식품', '직장인', '블로그', 150, '호감형');
    insert into influencer values('염아정', '30대', '7년', 2, '출산/육아', '주부', '블로그', 200, '건강');
    insert into influencer values('김준형', '40대', '3년', 3, '뷰티', '남성', '블로그', 200, '지적인');
    insert into influencer values('김혜성', '50대', '3년', 2, '식품', '자취생', '블로그', 100, '코믹');
    insert into influencer values('김민건', '30대', '3년', 3, '스포츠/레저/자동차','싱글','블로그', 150,'지적인');
    insert into influencer values('김연식', '10대', '3년', 3, '뷰티','학생','블로그',300,'호감형');
    insert into influencer values('최태열', '30대', '1년', 3, '펫','유부남','블로그',100,'코믹');
    insert into influencer values('조윤혜',  30대', '2년', 4, '펫', '싱글', '블로그', 150, '걸크러쉬');
    insert into influencer (name, target, year, influence, category, channel, cost, image) values ('장문희', '10대', '5년', 4, '식품', '블로그', 350, '트렌디');

    -- insert all로 입력
    insert all
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('고은비','30대','3년',3,'출산/육아','주부','인스타그램',150,'호감형')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('현준','40대','5년',2,'스포츠/레저/자동차','직장인','인스타그램',100,'코믹')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('최지원','20대','2년',1,'뷰티','직장인','인스타그램',100,'트렌디')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('권오민','20대','1년',3,'식품','자취생','인스타그램',100,'지적인')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('김재웅','전연령','6개월',2,'펫','','인스타그램',150,'호감형')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('김성호','30대','2년',2,'스포츠/레저/자동차','직장인','인스타그램',100,'건강')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('김창훈','30대','4년',2,'출산/육아','주부','인스타그램',120,'지적인')
            into influencer (name,target,year,influence,category,target_cha,channel,cost,image) values ('이정민','20대','3개월',2,'뷰티','학생','인스타그램',100,'걸크러쉬')
    select * from dual;
    ```

- INSTAGRAM

    ```sql
    -- instagram table 생성
    create table instagram(
        name varchar2(20) constraint instagam_name_fk references influencer(name),
        insta_id varchar2(50) not null,
        followers number(20),
        feeds number(10),
        insta_cost number(10)
    );

    --instagram table db 입력
    /* FK 제약조건이 있는 TABLE은 다중입력시에는 제약조건을 일시적으로 해제 해야함 
    alter table instagram disable constraint INSTARGAM_NAME_FK;
    alter table instagram enable constraint INSTARGAM_NAME_FK;
    */
    insert all 
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('고은비' ,'lovely_eunbi', 1500, 430, 100)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('현준', 'strong_jun', 1000, 350, 70)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('최지원', 'onevely_', 235, 230, 35)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('권오민', 'ohmin891', 80, 270, 60)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('김재웅', 'ganzi_ung', 80, 280, 80)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('김성호', 'cool726', 53, 150, 40)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('김창호', 'soft_chang537', 1200, 300, 90)
            into instagram (name, insta_id, followers, feeds,insta_cost) values ('이정민', 'mins_life', 15, 150, 30)
    select * from dual;
    ```

- YOUTUBE

    ```sql
    -- youtube table 생성
    create table youtube(
        name varchar2(20) constraint youtube_name_fk references influencer(name),
        youtube_id varchar2(50) not null,
        URL varchar2(50) not null,
        subscriber number(4) not null,
        youtube_cost number(4),
        avg_view number(10)
    );

    --youtube table db입력
    insert into youtube values('김연지', '찌양', 'https://www.youtube.com/channel/kyj', 200, 100, 40000);
    insert into youtube values('박다영', '냥이엄마', 'https://www.youtube.com/channel/pdy', 150, 50, 30000);
    insert into youtube values('최지수', '초이뷰티', 'https://www.youtube.com/channel/cjs', 800, 200, 150000);
    insert into youtube values('이민재', '어서와한국은처음이지', 'https://www.youtube.com/channel/lmj', 1, 30, 3000);
    insert into youtube values('장종욱', '관리하는남자', 'https://www.youtube.com/channel/jjw', 500, 300, 300000);
    insert into youtube values('박민영', '박민영 약사TV', 'https://www.youtube.com/channel/pmy', 40, 100, 10000);
    insert into youtube values('김혜경', '아는엄마', 'https://www.youtube.com/channel/khk', 3000, 1000, 1000000);
    insert into youtube values('차왕현', '크림히어로즈', 'https://www.youtube.com/channel/cwh', 300, 100, 500000);
    insert into youtube values('양호준', '헬보이양TV', 'https://www.youtube.com/channel/yhj', 1500,  500, 850000);
    ```

- BLOG

    ```sql
    -- blog table 생성
    create table blog(
        name varchar2(20) constraint blogname_fk references influencer(name),
        blog_ID varchar2(27) not null,
        neighbor number(10),
        visitors number(10),
        blog_cost number(10),
        powerblog varchar2(10) constraint powerblog_ck check (powerblog in ('O','X'))
    );

    -- blog table db 입력
    insert into blog values ('권희성', 'hs1997', 3957, 10298, 150, 'O');
    insert into blog values ('염아정', '귀엽수다', 2925, 17098, 200, 'O');
    insert into blog values ('김준형', '어른왕자', 5305, 10467, 200, 'O');
    insert into blog values ('김혜성', '평생공주절대미남자', 4002, 25930, 100, 'X');
    insert into blog values ('김민건', '피에스타', 5038, 5069, 150, 'O');
    insert into blog values ('김연식', '버터', 3047, 20940, 300, 'O');
    insert into blog values ('장문희', '무니무니', 10294, 29480, 350, 'O');
    insert into blog values ('최태열', '애드립', 1027, 10294, 100, 'X');
    insert into blog values ('조윤혜', '라임봉봉', 1570, 8093, 150, 'O');
    insert into blog values ('김혜경', '아는엄마', 307, 2098, 100, 'X');
    insert into blog values ('차왕현', '크림히어로즈', 809, 709, 100, 'X');
    insert into blog values ('양호준', '헬보이양', 270, 709, 100, 'X');
    ```
