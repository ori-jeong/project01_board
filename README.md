# Hello! I'm a Junior developer 👋😊
> 안녕하세요. 새로운 기술 개발로 꾸준히 성장하는 것을 목표로 달리고 있는 신입 개발자 진정희 입니다.  
> 3D 그래픽 개발자에서 웹 프로그래머로 새로운 길을 만들어 나가는 중인 만큼 
> 도전을 멈추지 않고 긍정적인 마인드로 소통하는 개발자가 되겠습니다.


# Project01. 개발일지💻
> 2021.06  
> 개발인원 : 1명
> 
> 'boardbell'은 '소통을 전하다'라는 의미로 모든 유저들이 자유롭게 글을 쓰고 취향을 찾아  
> 그것을 즐길수 있는 공간을 만들어 삶에 소소한 재미를 제공하는 서비스를 지향하고 있습니다.

> **프로젝트 구현 주소**:yellow_heart: https://boardbell.shop/  
> **시연영상**:purple_heart: https://www.youtube.com/embed/UTd_lkEU2R8


## 🛠개발환경🛠
<img src="https://img.shields.io/badge/Java-ea0016?style=flat-square&logo=Java&logoColor=white&width=300"> <img src="https://img.shields.io/badge/Spring-f26822?style=flat-square&logo=Spring&logoColor=white"/> <img src="https://img.shields.io/badge/tomcat8.5-7ed321?style=flat-square&logo=ApacheTomcat&logoColor=white"/> <img src="https://img.shields.io/badge/Oracle-00aeef?style=flat-square&logo=Oracle&logoColor=white"/> <img src="https://img.shields.io/badge/HTML-9187ff?style=flat-square&logo=HTML5&logoColor=white"/>

<img src="https://img.shields.io/badge/CSS-ff6384?style=flat-square&logo=CSS3&logoColor=white"/> <img src="https://img.shields.io/badge/Javascript-9187ff?style=flat-square&logo=Javascript&logoColor=white"/> <img src="https://img.shields.io/badge/JSP-00aeef?style=flat-square"/> <img src="https://img.shields.io/badge/JSTL-3333ff?style=flat-square"/> <img src="https://img.shields.io/badge/MyBatis-9933cc?style=flat-square"/> <img src="https://img.shields.io/badge/Ajax-2bb24c?style=flat-square"/>

<img src="https://img.shields.io/badge/jQuery-179c7d?style=flat-square&logo=jQuery&logoColor=white"/> <img src="https://img.shields.io/badge/AWS-343434?style=flat-square&logo=AmazonAWS&logoColor=white"/> <img src="https://img.shields.io/badge/OpenWeather-ff6384?style=flat-square"/> <img src="https://img.shields.io/badge/WebSocket-3333ff?style=flat-square"/>


## 프로젝트 구성📖
* **DB 모델링**  
![DB](https://user-images.githubusercontent.com/61307201/124126911-1bc91c80-dab6-11eb-9926-3a0e49231428.JPG)

* **사용자 이용 구조도**  
![user](https://user-images.githubusercontent.com/61307201/124128652-f3dab880-dab7-11eb-8994-9ece147531c8.JPG)

* **관리자 이용 구조도**  
![admin](https://user-images.githubusercontent.com/61307201/124128622-ed4c4100-dab7-11eb-8434-6027fa55c207.JPG)


## 화면구성/ 기능구현✨

### 반응형 웹

* 접속하는 디스플레이 접속 기기의 화면 크기를 자동으로 변하게 반응형으로 제작하였습니다.
* **화면구성**
![responsive](https://user-images.githubusercontent.com/61307201/124218437-2d9dd480-db35-11eb-971e-27160dfceabc.gif)

* **기능구현**

------
### 메인화면

**1. 날씨정보:sunny:**  
> Geolocation API(현재 위치 정보)와 OpenWeather API(날씨 API)를 사용해 현재 내 위치의 3시간별 기온 그래프와 날씨 아이콘으로 사용자에게 날씨 정보를 제공합니다.
  * **화면구성**  
  ![weather](https://user-images.githubusercontent.com/61307201/124220162-61c6c480-db38-11eb-899d-36f01acca1b3.JPG)
  
  * **기능구현**
  	+ navigator.geolocation로 사용 가능한 브라우저인지 확인 후 현재 정보를 요청해 받아온 값의 위도와 경도값을 저장합니다.
	```java
	if(navigator.geolocation) { //사용 가능 여부
		navigator.geolocation.getCurrentPosition(position => {//위치 허용 여부
			const lat = position.coords.latitude;	//위도
			const lon = position.coords.longitude;	//경도
			getWeather(lat, lon);
		}, error => { //위치 정보 허용 X= 기본값: 서울
			getWeather(37.56826,126.977829);
		})
	} else { //사용 불가 브라우저= 기본값: 서울
		getWeather(37.56826,126.977829);
	}
	```
	
	+ 받아온 위도 경도 값을 REST로 값을 주고 받아 기온 그래프와 날씨 정보를 제공합니다.
	```java
	fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=`+lat+`&lon=`+lon
				    +`&appid=bafd431d2ed57c71b8ac678da1847fa3`)
		    .then(res => res.json())
		    .then(data => {
		    //표시할 시간대 설정, 날씨 아이콘 적용 생략
		    //그래프 위치 계산
			for(var i=0;i<5;i++){
				chartData[i]=Math.ceil(Math.abs(chartData[i])*100)/100;
				if(chartData[i]<100){
					chartData[i]=Math.ceil((chartData[i]-30)*100)/100;
				}   		
			}
			//그래프선
			var dUrl="M17,"+chartData[0]+" L102.5,"+chartData[1]+" L189,"+chartData[2]
					+" L275.5,"+chartData[3]+" L360.5,"+chartData[4];
			document.getElementById("weather_graph").setAttribute("d",dUrl);
			document.getElementById("weather_ul").innerHTML=str;
			//$("#weather_ul").append(str);

			//그래프 점 위치
			var dotBottom=document.getElementsByClassName("weather_item");
			for(var i=0;i<dotBottom.length;i++){  
				dotBottom[i].style.top = (chartData[i]-15)+"px";    		
			}
		    })
	```
	
**2. 메인 메뉴/ 커뮤니티 메뉴/ 인기 게시판**
> 불필요한 이동 경로를 줄이기 위해 제공되는 서비스로 커뮤니티 메뉴와 인기 게시판은 DB연결을 통해 값을 가져옵니다.
  * **화면구성**  
  ![image](https://user-images.githubusercontent.com/61307201/124342619-111a9e80-dc00-11eb-8d42-2fe98b9ee08c.png)
  
  * **기능구현**
  	* HomeController: 커뮤니티 메뉴와 인기게시물을 model에 담아 출력합니다.
	```java
	 @RequestMapping(value = "/", method = RequestMethod.GET)
		public String home(SearchCriteria cri,Model model,Principal principal){
			logger.info("메인 페이지");	
			try {
				//커뮤니티 메뉴(일상,뷰티 등)
				model.addAttribute("brdNameList",service.brdNameList("20000"));							
				//인기 게시물 출력
				model.addAttribute("list",service.mainHotList());						
			}catch(Exception e){
				System.out.println(e.getMessage());
			}
			return "home";
		}
	```

	* boardMapper.xml: 커뮤니티 메뉴, 인기 게시물수 최대 5개를 불러옵니다.
	```java
	//메인 커뮤니티 메뉴
		<select id="brdNameListNon" resultType="hashmap">
			select * from board_list where brd_idx!=100
		</select>
	//메인 인기 게시물 수(최대 5개)
		<!-- hot 게시판 게시물수 -->
			<selelct id="mainHotList" resultType="hashmap">
			<![CDATA[select post_Idx,brd_Idx,post_title from(
				select  bp.*,(select count(*)from reply_20000 rp where bp.post_idx=rp.post_idx) reply_cnt 
				from board_post_20000 bp order by post_idx desc)
			where post_notice='N' and post_hit>=50 or reply_cnt>=50 and rownum <=5]]>
		</selelct>
	```
	
------	
### 회원가입
**1. 유효성 체크 및 안내**
> 아이디, 비밀번호, 닉네임, 이메일, 전화번호에 유효성 조건을 설정해 조건에 맞게 입력 할 수 있도록 도와줍니다.
  * **화면기능**
  * **기능구현**
  	+ member.js: 유효성 조건을 비교해 안내 문구를 출력합니다.	
	```java
	//아이디
	var join_id = document.getElementById("join_id");
	var id_regul2 = /^[-_0-9a-zA-Z]([-_]?[0-9a-zA-Z-_]){4,10}$/;
	join_id.onkeyup= function(){
		if(!id_regul2.test(join_id.value)){
			 if((join_id.value)==''){
				 id_regul.innerHTML="필수 입력입니다.";
			 }else{
				 id_regul.innerHTML="5~10자의 영문 대 소문자, 숫자와 특수기호(_),(-)만 사용 가능합니다."; 
			 }
			 id_regul.classList.remove('green');
	```

**2. 중복 확인**
> 아이디, 이메일, 닉네임 입력시 DB 데이터와 비교해 중복 확인을 합니다.
  * **화면기능**
  * **기능구현 (아이디만 작성)**
	+ member.js: 아이디, 이메일, 닉네임 입력시 DB 데이터와 비교해 중복 확인을 ajax 비동기식으로 구현했습니다.
	```java
	$.ajax({
		url: "/member/id_check",
		type: "POST",
		data: {"userId":(join_id.value)},
		datatype:"json",
		success:function(data){
			if(data==0){
				id_regul.classList.add('green');
				id_regul.innerHTML="사용 가능한 ID입니다.";
			}else{
				id_regul.innerHTML="이미 사용중이거나 탈퇴한 아이디입니다.";
				id_regul.classList.remove('green');
			}
		},error : function(errThrown){
			console.log("err",errThrown);
		}
	})
	```
	
	+ MemberController
	```java
	//아이디 중복체크
	@PostMapping("/id_check")
	public @ResponseBody int idCheck(String userId) throws Exception{
		logger.info("아이디 중복 체크");
		int idCheck=0;
		try {
			idCheck=service.idCheck(userId);
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}
		return idCheck;
	}
	```
	
	+ memberMapper.xml: 입력한 아이디 값이 있으면 1, 없으면 0을 출력합니다.
	```java
	<!-- 아이디 중복체크 -->
	<select id="idCheck" resultType="int">
		select count(*) from member_info where mem_id=#{userId}
	</select>
	}
	```

**3. 이메일 인증 코드**
> 인증코드 생성후 입력한 이메일로 인증코드를 전송해 인증 확인합니다.
  * **화면기능**
  * **기능구현**
 	 * MemberService: 코드생성 및 이메일 전송을 합니다.
	```java
	//이메일코드생성
	@Override
	public String creatCode() {
		Random  ran = new Random();
		StringBuffer sb = new StringBuffer();
		do {
			int num = ran.nextInt(75)+48;
			if((num>=48 && num<=57) || (num>=65 && num<=90) || (num>=97 && num<=122)) {
				sb.append((char)num);
			}else if(num>=47){sb.append((int)num);
			}else {continue;}
		} while (sb.length() < 7);
		return sb.toString();
	}
	//회원가입 이메일전송
	@Transactional
	@Override
	public void sendEmailCode(String code,String userEmail) {
		try {
			MailHandler mailHandler = new MailHandler(MailServiceImpl);
			//받는 사람
			mailHandler.setTo(userEmail);
			//보내는 사람
			mailHandler.setFrom("BoardBell");
			//제목
			mailHandler.setSubject("[BoardBell] 회원가입 인증코드 입니다.");
			//내용
			mailHandler.setText(
				new StringBuffer().append("<p>..생략..<span style=\"font-size: 18pt;\"> 인증번호 :&nbsp;"
						+ "<b>"+code+"</b></span></p>").toString());
			mailHandler.send();
		}catch(Exception e) {
			e.printStackTrace();
		}		
	}
	```
	 * root-context.xml: localhost로 실행할 땐 구글 아이디/ 비밀번호 입력으로 이메일이 보내지지만 aws 서비스시 보안 문제로 메일 전송이 안되므로 계정 2차 인증을 한 후 <span style="color:blue">2차 비밀번호</span>를 적어야 합니다.
	 ```java
	 <!-- 메일 보내기 (SEND) -->
	   <bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl">
	      <property name="port" value="465" />
	      <property name="username" value="EMAIL_ADDRESS" /> <!-- 구글 아이디 -->
	      <property name="password" value="EMAIL_PASSWORD" /> <!-- 구글 2차 비밀번호 -->
		  <property name="defaultEncoding" value="UTF-8" />
	      <!-- email 요청시는 SMTP, 보안연결 SSL 설정-->
	      <property name="javaMailProperties">
		 <props>
			<prop key="mail.smtp.host">smtp.gmail.com</prop>
			<prop key="mail.smtp.socketFactory.port">465</prop>
			<prop key="mail.smtp.auth">true</prop>
			<prop key="mail.smtp.starttls.enable">true</prop>
			<prop key="mail.smtp.debug">true</prop>
			<prop key="mail.smtps.ssl.trust">*</prop>           
			<prop key="mail.smtp.socketFactory.class">javax.net.ssl.SSLSocketFactory</prop>
			<prop key="mail.smtp.socketFactory.fallback">false</prop>
		 </props>
	      </property>
	   </bean>
	 ```
 
 
**4. 회원가입 처리**
> 입력된 회원 정보중 비밀번호는 암호화 처리를 한 후 DB에 저장해 회원가입을 완료합니다.
  * **화면기능**
  * **기능구현**
 	 * MemberController: form 안에 입력된 정보를 MemberVO로 받아옵니다.
   	```java
	/회원가입POST
	@PostMapping(value="/join", produces = "text/html; charset=utf8")
	public String postJoin(MemberVO vo,RedirectAttributes rttr) throws Exception{
		logger.info("회원가입 처리");
		int result=0;
		try {
			result=service.join(vo);
			if(result==1) {
				rttr.addFlashAttribute("joinMsg","success");
			}			
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}		
		return "redirect:/";
	}
	```  
 	 * MemberServiceImpl: BcryptPasswordEncoder로 입력한 비밀번호를 암호화해 MemberVO에 주입한 후 memberDAO로 전송합니다.
	```java
	//회원가입/비밀번호 암호화
	@Override
	public int join(MemberVO vo) throws Exception {
		String endcodedPassword = bcryptPasswordEncoder.encode(vo.getUserPw());
		vo.setUserPw(endcodedPassword);	
		return memberDAO.join(vo);
	}
	```  
 	 * memberMapper.xml: DB에 MemberVO 값을 저장합니다.(#{}='값'/ ${}=값, jdbcType=VARCHAR: 파라미터값이 null인걸 허용함)
   	```java
	<!-- 회원가입 -->
	<insert id="join" parameterType="MemberVO">
		insert into  member_info (mem_id,mem_pw,mem_nickname,mem_phoneNum,mem_postcode,mem_roadAddr,mem_detailAddr,mem_email,
		mem_regdate,mem_role)
		values (#{userId},#{userPw},#{userNickName},#{phoneNum,jdbcType=VARCHAR},#{postCode,jdbcType=VARCHAR},
		#{roadAddr,jdbcType=VARCHAR}, #{detailAddr,jdbcType=VARCHAR},#{email},sysdate,'ROLE_MEMBER')
	</insert>
	``` 
------	
### 로그인/ 로그아웃/ 계정찾기
> Security를 이용하여 로그인, 자동 로그인을 구현했으며 계정이 존재하지 않거나 틀리면 안내 문구가 생성되며
> 
**1. Security를 이용한 로그인/ 로그아웃/ 자동로그인**
  * **화면구현**
  * **기능구현**
	*  MemberController: 로그인 페이지 접속시 이전 접속 경로 담은 세션을 생성합니다. 
	```java
	@GetMapping(value="/login")
	public void getLogin(HttpServletRequest request) throws Exception{
		logger.info("로그인 페이지");
		try {
			String referrer = request.getHeader("Referer");
			if(!referrer.contains("/member/")){ //이전 경로에 /member/가 없으면 저장
				request.getSession().setAttribute("prevPage", referrer); 
			}
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}	
	}
	```
  	* login.jsp: 자동로그인 사용할 때 토큰 값을 같이 전송해야하기 때문에 form 전송 주소 값에 토큰값을 추가해서 합니다.
  	```java
	<form name="loginForm" method="post" action="/member/login-processing?${_csrf.parameterName}=${_csrf.token}" 
			onsubmit="login(document.loginForm); return false;" >
	```
   	* security-context.xml
	```java
	<!-- 로그인:  로그인 인증, 성공, 실패 처리 작업-->
		<security:form-login 
			login-page="/member/login"
			username-parameter="userId" password-parameter="userPw"
			login-processing-url="/member/login-processing"
			authentication-failure-handler-ref="userLoginFailHandler"
			authentication-success-handler-ref="userLoginSuccessHandler"
			always-use-default-target="true" />
	<!-- 로그아웃: 로그아웃 성공, 쿠키 삭제 작업-->
 		<security:logout logout-url="/member/logout"
			logout-success-url="/"
			invalidate-session="true"
			delete-cookies="remember-me,JSESSIONID" />
	<!-- 자동 로그인: remember-me 쿠키 브라우저에 저장, persistent_logins DB에 저장, 유효 기간: 7일 -->
	<!-- 사이트 재방문시 서버가 쿠키에 해당 정보를 DB에서 불러오면서 인증 유지 -->
		<security:remember-me remember-me-parameter="remember-me"  remember-me-cookie="remember-me" data-source-ref="dataSource" 
   			 authentication-success-handler-ref="userLoginSuccessHandler"  token-validity-seconds="604800" />
	``` 
  	 * UserLoginAuthProvider: 로그인 정보와 DB 정보를 비교합니다.
	```java
	@Override
	public Authentication authenticate(Authentication authentication) throws AuthenticationException {
		/* 사용자가 입력한 정보 */
		String userId = authentication.getName();
		String userPw = (String) authentication.getCredentials();

		/* DB에서 가져온 정보 */
		UserDetailsVO userDetails=new UserDetailsVO();
		try { userDetails = (UserDetailsVO)memberDao.getUserInfo(userId);
		} catch (Exception e) { e.printStackTrace(); }
		/* 인증 진행 */	
		if(userDetails==null ) { //계정이 없을때
			throw new UsernameNotFoundException(userId);
		}else if (!userId.equals(userDetails.getUsername()) //id 또는 pw가 다를경우
				|| !pwEncoding.matches(userPw, userDetails.getPassword()) ) {
			throw new BadCredentialsException(userId);		
		} else if (!userDetails.isEnabled()|| userDetails.getRole().equals("ROLE_STOP")) {
			throw new DisabledException(userId); //비활성화된 계정일 경우
		}
		//생략..
		// 다 썼으면 패스워드 정보는 지워줌 (객체를 계속 사용해야 하므로)
		userDetails.setUserPw(null);			
		/* 최종 리턴 시킬 새로만든 Authentication 객체 */
		Authentication newAuth = new UsernamePasswordAuthenticationToken(
				userDetails, null, userDetails.getAuthorities());
		return newAuth;
	}
	``` 
	* CustomUserDetailsService: DB에서 유저 정보를 가져옵니다.
	```java
	@Override
	public UserDetails loadUserByUsername(String userId) throws UsernameNotFoundException{
		//최종리턴할객체
		UserDetailsVO userVO=new UserDetailsVO();
		//사용자정보
		MemberVO mem =new MemberVO();				
		try { mem=memberDAO.getUserInfoDetail(userId);
		} catch (Exception e) { e.printStackTrace(); }
		//시큐리티 사용자 인증
		if(mem==null) {
			System.out.println("계정정보가 존재하지 않습니다.");
			return null;
		}else {
			userVO.setUserId(mem.getUserId());
			userVO.setUserNickName(mem.getUserNickName());
			userVO.setLink(mem.getLink());
		}	
		return userVO;
	```
	* UserLoginSuccessHandler: 로그인 성공시 이전 페이지로 이동합니다.
	```java
	@Override
	public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
			Authentication authentication) throws ServletException, IOException {
		HttpSession session = request.getSession();
		if (session != null) {
			//session에 저장된 이전경로를 String 변수로 저장
			String redirectUrl = (String) session.getAttribute("prevPage");				
			session.removeAttribute("prevPage");	//session에 있는 이전경로 삭제
			String link = (String) session.getAttribute("link");			
			if(link!=null) {
				session.removeAttribute("link");
				response.setContentType("text/html; charset=UTF-8");
				PrintWriter out=response.getWriter();
				out.println("<script>opener.location.reload();" + 
							"    opener.location.href=\""+redirectUrl+"\"; self.close();</script>");
				out.flush();
			}else {
				getRedirectStrategy().sendRedirect(request, response, redirectUrl);
			}
		} else {
			super.onAuthenticationSuccess(request, response, authentication);
		}
	}
	```
	* UserLoginFailHandler: 로그인 실패시 실패 안내 문구와 함께 로그인 페이지로 이동합니다.
	```java
	@Override
	public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,
			AuthenticationException exception) throws IOException, ServletException {
		if (exception instanceof AuthenticationServiceException) {
			request.setAttribute("loginFailMsg", "존재하지 않는 사용자입니다.");		
		} else if(exception instanceof BadCredentialsException) {
			request.setAttribute("loginFailMsg", "아이디 또는 비밀번호가 틀립니다.");
			
		} //생략..
		// 로그인 페이지로 이동
		request.getRequestDispatcher("/member/login").forward(request, response);		
	}
	```
	* UserDeniedHandler: 로그인 후 뒤로가기시 로그인 페이지가 아닌 메인페이지로 이동합니다.
	```java
	@Override
	public void handle(HttpServletRequest request, HttpServletResponse response,
			AccessDeniedException ade) throws IOException, ServletException {
		request.setAttribute("errMsg",ade.getMessage());
		request.getRequestDispatcher("/").forward(request,response);				
	}
	```
	* UserDeniedHandler: 로그인 후 뒤로가기시 로그인 페이지가 아닌 메인페이지로 이동합니다.
	```java
	@Override
	public void handle(HttpServletRequest request, HttpServletResponse response,
			AccessDeniedException ade) throws IOException, ServletException {
		request.setAttribute("errMsg",ade.getMessage());
		request.getRequestDispatcher("/").forward(request,response);				
	}
	```

**2. 소셜로그인**
> 네이버, 카카오 계정을 통해 'BoardBell' 홈페이지 이용에 필요한 정보를 받아와 가입 및 로그인 인증을 실행합니다.
> 
  * **화면구현**
  * **기능구현**
	*  MemberController: 네이버 로그인 성공시 인증 토큰값을 가져와 사용자 정보를 받아 가입여부를 확인한 후 로그인을 진행한다.
	```java
	@RequestMapping(value = "/naver_callback", method = { RequestMethod.GET, RequestMethod.POST })	
	public String callback(HttpServletRequest request,Model model, @RequestParam String code, 
			@RequestParam String state, HttpSession session,RedirectAttributes rttr) throws Exception {		
		String serverUrl = request.getScheme()+"://"+request.getServerName(); //현재페이지주소값
		if(request.getServerPort() != 80) {
			serverUrl = serverUrl + ":" + request.getServerPort();
		}
		OAuth2AccessToken oauthToken=naverAuthBO.getAccessToken(session, code, state,serverUrl); //인증토큰값 가져오기
		if(oauthToken == null) {
			model.addAttribute("msg", "네이버 로그인 access 토큰 발급 오류 입니다.");
			model.addAttribute("url", "/");
			return "/";
		}
		
		String apiResult = naverAuthBO.getUserProfile(oauthToken,serverUrl); //사용자 정보 가져오기
		JSONParser jsonParser = new JSONParser();
		Object obj = jsonParser.parse(apiResult);
		JSONObject jsonObj = (JSONObject) obj;		
		JSONObject response_obj = (JSONObject) jsonObj.get("response");

		// 프로필 조회
		String id = (String) response_obj.get("id");
		String email = (String)response_obj.get("email");
		String pw=id.substring(2,10);	
		int join=service.idCheck(id.substring(0,5)); //회원가입여부
		if(join==0) { //회원가입		
			Map<String,String> map=new HashMap<String,String>();
			map.put("userId",id.substring(0,5));
			map.put("userPw",pw);
			map.put("email",email);
			map.put("nick","nv_"+id.substring(0,5));
			map.put("link","naver");
			service.linkLogin(map);
		}
		session.setAttribute("link", "link");
		//로그인
		rttr.addFlashAttribute("id", id.substring(0,5));
		rttr.addFlashAttribute("pw", pw);
		return "redirect:/member/callback_login";
	}
	```
  	* NaverAuthBO: 네아로 인증 URL 생성, 인증토큰 가져오기, 사용자 정보 
  	```java
	//네아로 인증 URL 생성
	public String getAuthorizationUrl(HttpSession session,String serverUrl) { 
		String state = generateRandomString(); //세션 유효성 검증을 위하여 난수를 생성
		setSession(session, state); //생성한 난수 값을 session에 저장
		//Scribe에서 제공하는 인증 URL 생성 기능을 이용하여 네아로 인증 URL 생성 
		OAuth20Service oauthService = new ServiceBuilder()
				.apiKey(CLIENT_ID).apiSecret(CLIENT_SECRET) .callback(serverUrl+REDIRECT_URI)
				.state(state).build(NaverLoginApi.instance());  // 앞서 생성한 난수값을 인증 URL생성시 사용함 	
		return oauthService.getAuthorizationUrl(); 
	} 
	
	//네아로 Callback 처리 및 AccessToken 획득
	public OAuth2AccessToken getAccessToken(HttpSession session,String code, String state,String serverUrl)throws IOException { 
		//Callback으로 전달받은 세선검증용 난수값과 세션에 저장되어있는 값이 일치하는지 확인
		String sessionState = getSession(session); 
		if (StringUtils.equals(sessionState, state)) { 
			OAuth20Service oauthService = new ServiceBuilder()
					.apiKey(CLIENT_ID).apiSecret(CLIENT_SECRET) 
					.callback(serverUrl+REDIRECT_URI).state(state).build(NaverLoginApi.instance()); 
			//Scribe에서 제공하는 AccessToken 획득 기능으로 네아로 Access Token을 획득
			OAuth2AccessToken accessToken = oauthService.getAccessToken(code); 
			return accessToken; 			
		}
		return null;
	} 
	//Access Token을 이용하여 네이버 사용자 프로필 API를 호출
	public String getUserProfile(OAuth2AccessToken oauthToken, String serverUrl) throws IOException { 
		OAuth20Service oauthService = new ServiceBuilder()
				.apiKey(CLIENT_ID).apiSecret(CLIENT_SECRET) 
				.callback(serverUrl+REDIRECT_URI).build(NaverLoginApi.instance()); 
		OAuthRequest request = new OAuthRequest(Verb.GET, PROFILE_API_URL, oauthService);;
		oauthService.signRequest(oauthToken, request); 
		Response response = request.send();
		return response.getBody(); 
	}
	```


**3. 아이디 찾기/ 비밀번호 찾기**
> 아이디 찾기: 가입한 이메일을 입력해 해당 계정의 아이디를 보여줍니다.  
> 비밀번호 찾기: 가입한 아이디와 이메일 입력 후 가입한 이메일로 임시 비밀번호를 전송합니다. 코드 전송 후 코드를 암호화해 해당 회원의 비밀번호 DB를 수정합니다.  
>  
  * **화면구현**
  * **기능구현**
  	* MemberController: 이메일 또는 이메일, 아이디 값을 받아 비교 후 결과 페이지로 이동 시 아이디 출력 또는 임시 비밀번호 전송합니다.
	```java
	@PostMapping("/find_id_pw")
	public @ResponseBody int findId(MemberVO vo) throws Exception{
		logger.info("아이디 / 비밀번호 찾기를 위한 정보 비교 처리");
		int findId=0;
		try { findId=service.findIdPw(vo);
		}catch(Exception e) { System.out.println(e.getMessage());}	
		return findId;
	}
	@PostMapping("/resultIdPw")
	public void resultIdPage(MemberVO vo,Model model) throws Exception{
		logger.info("id값 출력 및 임시비밀번호 발급");
		try {
			if(vo.getUserId()==null) {
				model.addAttribute("idText",service.resultId(vo));
				model.addAttribute("result","id");
			}else {
				//임시비밀번호 생성
				String code=service.creatCode();
				//코드전송
				service.sendEmailPw(code,vo.getEmail());
				model.addAttribute("result","pw");	
			}
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}			
	}
	```
	* MemberMapper.xml: 입력한 아이디와 이메일이 일치한 회원의 비밀번호를 임시 발급한 비밀번호(암호화 됨)로 변경합니다.
	```java
	<!-- 비밀번호 -> 임시 비밀번호로 변경 -->
	<update id="changePw" parameterType="map">
		update member_info set mem_pw=#{pw} where mem_email=#{email}
	</update>
	```
-----

### 공지
* 사용자별 권한  
   |사용자|글 읽기|글 쓰기|글 수정|글 삭제|
   |:----:|:----:|:-----:|:-----:|:-----:|
   |비회원|O|X|X|X|
   |회원|O|X|X|X|
   |관리자|O|O|O|O|

 * 글쓰기
> 전체공지: 전체 공지 체크 후 글 작성 완료하면 공지 및 커뮤니티 페이지 전체에 공지가 등록됩니다.  
> 공지글: 공지 게시판의 공지글로 등록됩니다.
  * **화면구현**
  * **기능구현**
   	* 
	```java
	
	```

### 커뮤니티
* 사용자별 권한  
|사용자|글 읽기|글 쓰기|공지글 쓰기|글 수정|글 삭제|
|:----:|:----:|:----:|:----:|:----:|:----:|
|비회원|O|X|X|X|X||
|회원|O|O|x|O|O||
|관리자|O|O|O|O|O||

1. 전체
   * 각 게시물의 게시판명 표시합니다.
   * 공지 리스트는 전체 공지 목록만 보여줍니다.(최대 5개)
 
2. 인기글
   *  조회수 50 이상 또는 댓글 50개 이상인 글만 인기글 리스트에 표시됩니다.

3. 게시판/ 카테고리
   * 게시판 안에 카테고리로 세분화 가능하며 각 글에 카테고리명을 표시합니다.
   * 각 게시판, 카테고리 공지글을 작성할 수 있습니다.

### 글쓰기
1.공지글 체크
   * 관리자 권한만 사용 가능하며 '전체','인기글' 메뉴을 제외한 모든 게시판에서 사용 가능합니다.
2. 게시판/ 카테고리 목록
   * 전체/ 인기글에서 '글쓰기' 버튼 클릭시 커뮤니티에 존재하는 모든 게시판 목록을 보여줍니다.
   * 게시판 선택은 필수입니다.
   * 게시판 선택 시 해당 게시판의 카테고리를 출력합니다.
   * 카테고리는 선택사항입니다.
3. 글쓰기 에디터
   * 폰트, 크기, 굵기, 정렬 등의 기능을 사용해 게시글을 꾸밀 수 있습니다.
4. 파일첨부
   * 첨부 아이콘 클릭 시 내 컴퓨터내에 존재하는 파일을 첨부할 수 있습니다.
   * 첨부 가능한 파일은 2MB이하, 확장자 .exe,.sh,.alz를 제외한 파일입니다.
   * 첨부한 파일은 Amazon S3에 저장됩니다.

### 글수정
* 수정을 원하는 글이 공지글이면 공지글 체크되어 표시되고 게시판, 카테고리, 제목과 내용, 첨부파일이 표시됩니다.


### 답글 쓰기
* 공지글 체크가 표시되지 않아 공지글로 등록할 수 없습니다.
* 게시판을 바꿔서 작성할 순 없지만 카테고리는 수정 가능합니다.

### 게시판 공통기능
#### 게시판 목록 페이지
   1. 공지 숨기기 
      * 전체공지와 게시판의 공지를 숨길 수 있습니다.

   2. 목록 개수
      * 페이지에 보여줄 게시물 수를 설정할 수 있습니다.(기본 설정: 10개씩)

   3. 게시물 번호
      * 글 번호는 해당 게시판/ 카테고리의 총 게시물 수의 역순으로 보여줍니다.

   4. 댓글 개수 
      * 시물 댓글 수가 1개 이상일 때 제목 옆에 표시됩니다.

   5. 답글 구분
      * └RE: 표시로 답글 게시물을 구분할 수 있습니다.

   6. 게시물 날짜
      * 당일 올린 게시물은 올린 시간으로 표시되며 다음날이 되면 날짜로 보여줍니다.

   7. 페이징
      * 한 페이지의 목록 출력 개수 초과시 다음 페이지 번호를 생성해 다음글을 볼 수 있습니다.
      * 이지 10개 초과 시 다음> 버튼 생성, 현재 페이지가 11 이상이면 <이전 버튼 생성

   8. 검색
      * 색타입(제목+내용, 제목, 글작성자)에 맞게 해당 게시판 내에서 검색이 가능합니다.

#### 게시물 페이지
   1. 이전글/ 다음글
      * 현재 게시판 내 이전글과 다음글로 이동합니다.
      
   2. URL복사
      * 해당 게시물의 주소가 복사됩니다.

   3. 첨부파일 목록/ 다운로드
      * 첨부된 파일 목록이 표시되며 클릭시 다운로드 됩니다.

   4. 게시물 댓글 목록
      * 해당 게시물의 총 댓글수가 표시되며 내가 쓴 게시물

### 채팅
> WebSocket을 사용해 다자간 채팅 기능을 제공합니다.
* 채팅방 입장 전
   + 비회원: 로그인/ 회원가입 안내하는 문구와 버튼을 표시합니다.
   + 회원: 닉네임 입력 후 채팅방 입장이 가능합니다.
* 채팅방 입장 후
   + 현재 채팅방에 접속된 인원 수를 표시합니다.
   + 입장과 퇴장을 알리는 알림문구가 뜹니다.
   + 나 이외의 유저 채팅글은 왼쪽정렬로 출력해 내 채팅과 구분하여 보여집니다.



### 마이페이지
   1. 회원정보수정
      + 비밀번호, 닉네임, 이메일, 전화번호, 주소를 수정할 수 있습니다.
   2. 게시물관리
      + 내가 쓴 게시물 제목을 확인할 수 있으며 선택 후 삭제 가능합니다.
   3. 댓글관리
      + 내가 쓴 댓글 내용을 확인할 수 있으며 선택 후 삭제 가능합니다.
   4. 회원탈퇴
      + 탈퇴시 7일 이후 모든 정보가 삭제됩니다.
      + 그 전까진 계정의 아이디와 이메일, 닉네임을 사용할 수 없습니다. 


### 관리자 페이지
   1. 유저관리
      + 아이디 또는 닉네임으로 유저를 검색할 수 있습니다.
      + 총 유저 수, 아이디, 닉네임, 상태(권한), 가입일, 게시글수, 댓글수를 보여줍니다.
      + 유저를 선택 한 후 유저 상태를 변경할 수 있습니다(활동/ 활동정지)
      
   2. 메뉴관리
      + 게시판 메뉴, 게시판, 카테고리를 추가, 수정, 삭제할 수 있습니다.
      + 게시판 메뉴 삭제시 하위 게시판과 카테고리도 함께 삭제됩니다.
      + 게시판 삭제시 하위 카테고리도 함께 삭제됩니다.
### 기타기능
   * DB에 없는 첨부파일 자동 삭제
      + DB 파일 리스트 데이터와  AWS S3 파일 리스트를 비교해 DB에 없는 파일을 자동으로 삭제합니다.
      + 매일 23시 59분 59초에 실행
      
# 프로젝트 후기
> 초기 구상한 프로젝트는 게시판 기능과 회원가입, 로그인 정도였는데 여러 사이트들을 비교하면서 날씨, 채팅, 권한에 따른 기능, 관리자 페이지, AWS를 이용한 웹 서비스 제공 등 좀 더 다양한 기능을 고민하며 넣다 보니 생각보다 개발 기간이 오래 걸린 것 같습니다.  
> 하지만 javascript, SQL 등 전보다 쉽게 작성 할 수 있게 되었으며 웹 개발의 구조를 이해할 수 있는 계기가 되었습니다.

> Security 구조 파악과 DB정보를 하나의 Query문으로 작성해 출력, AWS를 사용해 웹 서비스를 제공하는 부분을 이해하고 구현하기 위해 같은 주제지만 조금씩 다른 코드들을 비교해나가면서 이해를 하면서 내 프로젝트에 더 맞는 코드로 작성할 수 있게 되었습니다.
> 
>아쉬었던 점은 Spring AOP 기능을 활용하지 못한 부분과 그림을 첨부해 보여주는 기능, 채팅방을 여러개 생성해 입장하는 기능을 구현하지 못한게 아쉬웠고, 또 혼자 개발한 점이 큰 것 같습니다. 기능과 코딩 개선을 혼자 고민하면서 개발 속도도 점점 느려지고, 다른 사람의 코드를 보고 소통할 수 있는 기회도 없어 내 코드에 지적할 사람이 없어서 이게 맞는 건가 싶은 생각이 많이 들었습니다. 그리고 협업을 위한 버전 관리 시스템(git)을 활용하지 못해 후에 고생을 좀 할 것 같다는 생각이 들었습니다.

> 부족한 부분이 많은 프로젝트이지만 배웠던 부분을 복습하고 새로운 기능을 공부를 할 수 있어서 좋았고, 결과물이 어느 정도 구색이 갖춰진 것 같아 내심 만족스러운 프로젝트였습니다. 부족하지만 끝까지 봐주셔서 감사합니다.
