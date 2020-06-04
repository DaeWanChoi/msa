## MSA QnA
내 나름대로 질문 목록을 만들고 해당 답변을 기록한다.

1. JPA Entity Join의 경우 DTO에서 JoinKey만 받아서 Entity로 셋팅을 할때 setter 함수를 쓰는 방법이 맞는가?
   - setter를 사용하지 않고 public void createUser(final User user) 등으로 명확한 목적과 의도를 나타낼 수 있는 메소드를 추가해서 사용
	  
2. 서비스 Test시 Repository return 값을 given으로 줄때 Before에서 넣어야 하는가?
   그렇게 되면 id를 조회할 때 자동증감인데 해당 생성자에 값을 set하도록 builder를 추가해야 하는가?
   - 각 Test에서 User user = mock(User.class); 으로 Entity에 대한 Mock을 만들고
   ```
   given(userRepository.save(any(User.class))).willReturn(user);
	 given(user.getId()).willReturn(1L);
   ```
	 처리하면 자동증감에 대한 Mock 처리를 할수 있고 서비스 로직 테스트에 집중할수 있다.
	  
3. MSA의 경우 다른서비스에서 사용자 정보테이블이 있고 현재 서비스에는 없을 경우 Join을 어떻게 걸어줘야 하는가?
   - A서비스가 B서비스에서 가지고 있는 테이블에 대하여 필요한 컬럼이 있다면 A서비스에서 B서비스의 필요 컬럼을 A서비스가 최소한으로 가지고 있는 형태로 구현하자. 그외에는 Kafka 같은 비동기 Queue를 활용하여 서비스간 데이터를 공유한다.
   
4. JpaRepository에서 Optional 처리 방법
   - 아래와 같이 Optional로 받아서 get 처리
   ```
     @Transactional(readOnly = true)
	 public <T, ID, C> C findById(T t, ID id, Class<C> type, ErrorCode code) {
	    final Optional<C> m = ((JpaRepository)t).findById((long)id);
	    m.orElseThrow(() -> new NotFoundException((long)id, code));
	    return type.cast(m.get());
	 }
	 ``` 
   
5. Entity에서 @JsonProperty와 @JsonIgnore 의미
   - @JsonIgnore = Json으로 변환시 보이지 않도록 처리
   
6. Restful DTO에서 @Valid 어노테이션에 대하여 GET으로 처리할 때 어떻게 하는가?
   - Get은 @Valid 처리를 하지 않는다.

7. JWT에서 로그아웃 처리방법은?
   - Client에서 토큰을 삭제
   - blanklist를 관리
   - 만료시간을 짧게 조정
   
* ETC
  - @Embedded 활용  
      Dto.Res에서 ModelMapper를 사용하려면 @Setter가 적용되어야 함
