### Next-Auth로 Google 소셜 로그인 구현하기 (Type A)

#### 키 값 관리
- **환경 변수 사용**: Google (Firebase)에서 발급한 Client ID와 Client Secret을 안전하게 관리하기 위해, `.env` 파일 등을 사용하여 환경 변수에 저장하세요.

```env
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
```

#### Redirect URI 설정
- **등록된 Redirect URI 확인**: Google Console에서 등록한 Redirect URI가 실제 사용되는 URI와 일치하는지 확인하세요. `redirect_uri_mismatch` 에러는 등록되지 않은 URI로의 리디렉션 시 발생합니다.

#### Callback URL 구성
- **콜백 URL 검증**: 콜백 URL에 들어오는 요청이 정상적인 로그인 검증을 위한 것인지 확인하는 로직을 추가하세요.

#### 에러 핸들링
- **오류 메시지 처리**: Google 로그인 시 발생하는 다양한 오류에 대한 적절한 핸들링을 구현하세요.

### useMediaQuery 사용을 통한 반응형 웹 구현 주의 사항 (Type A)

#### 불필요한 리렌더링 방지
- **useMemo 활용**: `useMemo`를 사용하여 불필요한 리렌더링을 최소화하세요.

### Tailwind CSS에서의 반응형 웹 구현 (Type A)

#### Breakpoint
- **기본 Breakpoint 활용**: Tailwind CSS는 기기 해상도를 기반으로 설정한 5개의 기본 Breakpoint를 제공합니다. 필요에 따라 `tailwind.config.js`에서 추가 설정이 가능합니다.

<img width="1000" alt="스크린샷_2023-09-21_오후_2 57 55" src="https://github.com/TaskerJang/ASAC-3rd-CodeReview/assets/124780552/e5583a77-2c24-4a27-ad14-8ff69637e729">


#### Breakpoint를 사용하는 방법
- **Class 작성**: Breakpoint에 대한 prefix를 먼저 작성하고 `:`를 입력한 다음, 적용할 클래스 명을 작성하세요.
  예: `md:w-32`

#### 유틸리티 클래스
- **재사용성 고려**: 유틸리티 클래스를 적절하게 활용하되, 과도하게 사용하지 않도록 주의하세요. 필요하다면 컴포넌트로 추상화하여 재사용성을 높이세요.
