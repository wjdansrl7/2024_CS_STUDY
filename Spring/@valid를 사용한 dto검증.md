# @valid를 사용한 dto검증

# @Valid란?

@Valid는 JSR-303 표준 스펙(자바 진영 스펙)으로써 Bean Validator를 이용하여 객체의 제약 조건을 검증하도록 지시하는 어노테이션이다. JSR 표준의 빈 검증 기술의 특징은 객체의 필드에 달린 어노테이션으로 편리하게 검증을 한다는 것이다.Spring에서는 LocalValidatorFactoryBean을 Bean으로 등록하면 LocalValidatorFactoryBean이제약 조건 검증을 처리한다. Spring Boot에서는 아래와 같이 의존성을 추가해주면 해당 기능들을 사용할 수 있다.

# @Valid 적용

### 의존성을 build.gradle에 추가해준다

```java
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

### 검증할 객체와 해당 객체의 제약사항을 추가
dto/MemberDto

```java
public class MemberDto {
	@Getter
	@Builder
	public static class MemberJoinRequest{
	    @Email
	    @NotBlank
	    @Size(max = 50, message = "50자 이하만 가능합니다.")
	    private String memberEmail;
	    @NotBlank
	    @Size(min = 8,max = 20, message = "8자 이상, 20자 이하만 가능합니다.")
	    private String memberPassword;
	    @NotBlank
	    @Size(min = 2,max = 20, message = "2자 이상, 20자 이하만 가능합니다.")
	    private String memberName;
	}
	
	@Getter
	@Builder
	public static class MemberUpdateRequest{
	    private int memberId;
	    @NotBlank
	    @Size(min = 8,max = 20, message = "8자 이상, 20자 이하만 가능합니다.")
	    private String memberPassword;
	}
}
```

- @Email : 이메일 형식인지 검증
- @NotNull : 해당 값이 null인지 검증
- @NotEmpty : 해당 값이 null이 아니고 비어있는 문자열인지 검증
- @NotBlank : 해당 값이 null이 아니고 비어있는 문자열 인지 공백 문자열인지 검증
- @AssertTrue : 해당 값이 true인지 검증
- @Size : 해당 값이 주어진 값 사이에 해당하는지 검증. 문자열, 배열 등의 크기를 검증하는데 사용.
- @Min, @Max : 각각 주어진 값보다 작지 않은지, 크지 않은지 검증
- @Pattern : 해당 값이 주어진 패턴과 일치하는지 검증

### 해당 객체를 파라미터로 받는 API에 @Valid 추가
controller/MemberController

```java
@PostMapping("/signup")
public ResponseEntity signup(@RequestBody @Valid MemberDto.MemberJoinRequest memberDto){
	Member member = memberService.createMember(memberDto);
	MemberDto.MemberAllResponse response = MemberDto.MemberAllResponse.builder()
		.memberId(member.getMemberId())
		.memberEmail(member.getMemberEmail())
		.memberName(member.getMemberName())
		.build();
return new ResponseEntity(success(response),HttpStatus.OK);
}
```

참고자료

[https://seungh1024.tistory.com/57](https://seungh1024.tistory.com/57)