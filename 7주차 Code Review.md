- (Type A) Next-Auth로 Google 소셜 로그인 구현하기
    - **키 값 관리**: Google 로그인을 구현하기 위해서는 Google (Firebase)에서 발급한 Client ID와 Client Secret이 필요로 합니다. 이 값을 저장할 때에는 절대 사용자가 볼 수 있도록 해서는 안되며, `.env` 파일 등 환경 변수로 이 키 값을 가져와서 구현해야 합니다.
    - **Redirect URI 설정**: Google 로그인을 진행할 때, 미리 등록되지 않은 Redirect URI로 이동하려고 하면 에러가 발생합니다. `redirect_uri_mismatch` 에러가 발생하는 경우, 이 부분에 대한 설정을 꼭 확인해보세요.
    - **Callback URL 구성**: Callback URL은 Google에서 로그인 성공 여부를 확인하기 위해 사용되는 URL입니다. 따라서 콜백 URL에 들어오는 요청들이 정상적인 접근 (로그인 검증)을 위해 사용된 것인지 검증하는 로직이 추가되면 좋습니다.
    - **에러 핸들링**: Google 로그인을 진행하면서 오류가 발생했을 경우, 오류에 따라서 적절한 핸들링은 필수입니다.
- (Type A) useMediaQuery 사용을 통한 반응형 웹 구현 주의 사항
    - **불필요한 리렌더링 방지**: 조건에 따라 크기가 변하면 컴포넌트가 불필요하게 렌더링되는 경우가 발생할 수 있습니다. `useMemo`와 같은 훅을 이용하여 불필요한 리렌더링을 최대한 방지하세요.
- (Type A) Tailwind CSS 에서의 반응형 웹 구현
    - **Breakpoint**: Tailwind CSS 에는 일반적으로 널리 사용되는 기기들의 해상도를 기반으로 설정한 5개의 기본 Breakpoint를 제공합니다. 이를 활용하면 불필요한 반복을 줄일 수 있으며, 만약 기본 제공 값으로 충분하지 않은 경우 `tailwind.config.js`와 같은 Tailwind CSS 설정 파일에서 추가로 작성하는 것도 가능합니다.
        - 기본 Breakpoint

          ![https://tailwindcss.com/docs/responsive-design](https://prod-files-secure.s3.us-west-2.amazonaws.com/ddf78827-42af-43b7-a69e-15559bce6dbf/eae87777-1839-4956-9eb4-e44e72cc24de/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-09-21_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.57.55.png)

          https://tailwindcss.com/docs/responsive-design

    - **Breakpoint를 사용하는 방법**: class를 작성할 때, Breakpoint에 대한 prefix를 앞에 먼저 작성하고 `:` 를 입력한 다음 그 뒤에 적용할 클래스 명을 작성하세요.
      예: `md:w-32`
    - **유틸리티 클래스**: 유틸리티 클래스를 너무 과도하게 많이 사용하게 되면 HTML의 내용이 너무 복잡해질 수 있습니다. 가능하다면 Component로 만들어 재사용성을 높여보세요.