# Table 만드는 규칙 (table => 항속적으로 존재하는 데이터)  
  
1. 컬럼이 없는 테이블은 존재할 수 없다.  
2. 테이블 안에 있는 인스턴스(row)는 구별 가능해야 한다.  
(unique, not null) => primary key(개체 무결성) (ex. auto_increment ; mysql)  
3. 테이블명과 일치하는 컬럼명은 만들지 않는다.  
4. 테이블의 이름은 단수명사를 사용한다.
</br></br>

# 데이터 무결성이 지켜져야 한다.  
1. 개체 무결성 (Default 제약조건)  
2. 도메인 무결성 (영역을 벗어나지 않는다.) (data type, check)  
3. 참조 무결성    foreign key (reference key)  
</br>무결성을 지키기 위해서 제약조건을 사용한다.
</br></br>
primary key 는 숫자인 것이 좋다.