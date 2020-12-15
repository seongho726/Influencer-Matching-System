# ISSUE

- **OUTER JOIN** 사용 실패

    ```sql
    select i.name, insta_id, youtube_id
    from influencer i, instagram g, youtube y
    where i.name=g.name
    group by i.name, insta_id, youtube_id
    ```

    단순 FROM절에 테이블을 3개이상 추가 시 결과 값

    ![ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_q1.png](ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_q1.png)

- **OUTER JOIN** 해결책

    ```sql
    select i.name, insta_id, youtube_id
            from influencer i
            left outer join instagram g on (i.name=g.name)
            left outer join youtube y on (i.name=y.name);
    ```

    ![ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_a1.png](ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_a1.png)

    **LEFT OUTER JOIN** 은 ****왼쪽 테이블의 모든 행과 오른쪽 테이블의 행을 비교, 결과가 없더라도 NULL값을 출력합니다. OUTER JOIN이 여러개 일치할 경우 중복 값이 나옵니다

- **FK 제약**이 걸린 TABLE의 다중입력 실패

    ![ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_q2.png](ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_q2.png)

- **FK 제약조건**이 걸린 TABLE의 다중입력 해결책

    ![ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_a2.png](ISSUE%201d0b8c7924524fff96d805079873e9d0/issue_a2.png)

    1. **FK 제약조건**이 있는 TABLE은 다중입력시에는 제약조건을 일시적으로 해제 해야함 
    alter table instagram disable constraint INSTAGAM_NAME_FK;

    2 . insert all 이 아닌 데이터 하나씩 추가 
     select * from dual;
    insert into instagram values ('고은비','lovely_eunbi',1500,430,100);
    insert into instagram values ('현준','strong_jun',1000,350,70);
    insert into instagram values ('최지원','onevely_',235,230,35);
    insert into instagram values ('권오민','ohmin891',80,270,60);
    insert into instagram values ('김재웅','ganzi_ung',80,280,80);
    insert into instagram values ('김성호','cool726',53,150,40);
    insert into instagram values ('김창훈','soft_chang537',1200,300,90);
    insert into instagram values ('이정민','mins_life',15,150,30);
    insert into instagram values ('김혜경','_hyevely_',1000,450,120);
