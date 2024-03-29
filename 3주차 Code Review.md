## `tsconfig.json` 설정시 주의해야 하는 부분

1. **옵션 충돌**: 여러 옵션을 설정할 때 충돌이 발생할 수 있습니다. 예를 들어, `target`과 `lib`을 동시에 설정할 때 서로 호환되지 않는 설정을 피해야 합니다.
   
3. **`strict` 옵션**: 가능한 경우 항상 `strict` 옵션을 활성화하여 타입 안정성을 강화하는 것이 좋습니다. 그러나 기존 프로젝트에 적용할 경우, 코드의 많은 부분에서 타입 오류가 발생할 수 있으므로 점진적으로 적용하는 것을 권장합니다.
   
5. **파일 및 디렉토리 관련 옵션**: `files`, `include`, 및 `exclude`는 프로젝트의 범위와 구조를 제어합니다. 올바르게 설정하지 않으면 의도하지 않은 파일이 컴파일되거나 중요한 파일이 제외될 수 있습니다.
   
7. **확장과 오버라이드**: `extends`를 사용하여 다른 설정 파일을 확장할 때, 기본 설정을 오버라이드할 수 있습니다. 이로 인해 예상치 못한 동작이 발생할 수 있으므로 주의가 필요합니다.

---

## ESLint 설정시 주의해야 하는 부분

1. **규칙 충돌**: 다양한 설정을 확장하거나 여러 플러그인을 사용할 때, 규칙들 사이에 충돌이 발생할 수 있습니다.
   
3. **버전 호환성**: ESLint 플러그인이나 파서의 버전이 ESLint 자체의 버전과 호환되지 않을 수 있습니다. 항상 호환성을 확인하고 필요한 버전을 설치하세요.
   
4. **오버라이드**: `overrides` 섹션을 사용하면 특정 파일이나 디렉토리에 대한 규칙을 재정의할 수 있습니다. 이를 잘못 사용하면 예상하지 못한 동작이 발생할 수 있습니다.
   
6. **환경 설정**: `env` 설정을 잘못 지정하면, 실제 환경에는 존재하지 않는 전역 변수에 대한 오류를 놓칠 수 있습니다.
   
8. **과도한 규칙**: 너무 많은 규칙을 활성화하면 개발자의 생산성에 영향을 줄 수 있습니다. 규칙은 프로젝트나 팀의 필요에 따라 선택되어야 합니다.
   
10. **파서 옵션**: 잘못된 `parserOptions` 설정은 예상치 않은 ESLint 경고나 오류를 발생시킬 수 있습니다. 설정을 잘 이해하고 적용하세요.
    
12. **규칙 설정의 명확성**: `rules`에서 규칙을 설정할 때, 각 규칙에 대한 설명과 이유를 주석 또는 문서화하여 팀원들이 이해할 수 있게 해야 합니다.
    
14. **규칙의 세부 설정**: 일부 규칙들은 추가적인 옵션을 가질 수 있습니다. 규칙의 동작을 정확히 이해하고 설정해야 합니다.
    
16. **코드 형식과 코드 품질**: ESLint는 코드 형식과 코드 품질에 관한 규칙을 모두 제공합니다. 규칙을 설정할 때, 팀의 코딩 스타일과 기준을 고려하여 선택하세요.

---

## Prettier 설정시 주의해야 하는 부분

1. **ESLint와의 충돌**: ESLint와 Prettier를 함께 사용하는 경우, 충돌하는 규칙이 있을 수 있습니다. 이러한 문제를 해결하기 위해 `eslint-config-prettier`를 사용하는 것이 좋습니다.
   
3. **프로젝트 통일성**: 프로젝트의 모든 참여자가 동일한 Prettier 설정을 사용하는 것이 중요합니다. 설정 파일을 프로젝트 루트에 위치시켜 팀원 모두가 동일한 설정을 사용하도록 해야 합니다.
   
5. **버전 호환성**: Prettier의 새로운 버전이 출시될 때마다 설정 옵션이 변경될 수 있습니다. 항상 사용하는 Prettier의 버전과 설정의 호환성을 확인해야 합니다.
   
4. **자동 저장**: 일부 편집기 확장 프로그램은 파일을 저장할 때 자동으로 Prettier를 실행할 수 있습니다. 이는 원치 않는 변경이 발생할 수 있으므로, 처음 설정할 때 주의해야 합니다.
   
6. **VCS 충돌**: Prettier를 처음 도입하면 많은 파일이 변경될 수 있습니다. 이는 버전 관리 시스템(VCS)에서 충돌을 일으킬 수 있으므로, 큰 변경은 별도의 커밋으로 관리하는 것이 좋습니다.
   
8. **플러그인 활용**: Prettier는 다양한 언어에 대한 플러그인을 지원합니다. 필요한 언어나 도구에 대한 플러그인을 포함시키는 것을 잊지 마세요.

---

## Webpack에서 SVG 아이콘을 지원하도록 설정할 때 주의해야 하는 것들

Webpack에서 SVG 아이콘을 사용하려면, 적절한 로더와 설정을 활용해야 합니다. SVG를 처리하는 데 있어서 일반적인 접근 방법은 `file-loader`나 `url-loader`를 사용하는 것이지만, SVG를 React 컴포넌트로 가져오려면 `@svgr/webpack`과 같은 도구를 사용할 수도 있습니다.

1. **파일 크기**: SVG 파일의 크기가 큰 경우, `url-loader`를 사용하여 인라인으로 처리하면 번들 크기가 커질 수 있습니다. 이런 경우에는 `limit` 옵션을 사용하여 임계값을 설정하고, 그 이상의 크기를 가진 파일은 `file-loader`를 통해 별도의 파일로 처리하도록 설정하세요.
   
3. **보안 문제**: SVG는 실행 가능한 코드를 포함할 수 있으므로, 악의적인 SVG 코드가 애플리케이션에 포함되지 않도록 주의해야 합니다.
   
5. **SVG 최적화**: SVG 파일은 종종 불필요한 메타데이터나 주석을 포함할 수 있습니다. 이러한 불필요한 데이터를 제거하기 위해 `svgo`와 같은 도구를 사용하여 SVG를 최적화하세요.
   
7. **반응형 디자인**: SVG는 크기와 색상 등 다양한 속성을 조절할 수 있습니다. 반응형 디자인을 구현할 때 이러한 속성들이 올바르게 적용되고 있는지 확인하세요.
   
9. **스타일과 CSS**: SVG는 내부적으로 스타일을 포함할 수 있습니다. Webpack을 사용하여 CSS 모듈이나 스타일 로더와 함께 SVG를 처리할 때, 스타일 충돌이 발생하지 않는지 주의해야 합니다.
    
11. **브라우저 호환성**: 대부분의 현대 브라우저는 SVG를 지원하지만, 구버전의 브라우저에서는 문제가 발생할 수 있습니다. 필요한 경우에는 폴리필을 사용하여 호환성을 보장하세요.
    
13. **컴포넌트 변환**: SVG를 React 컴포넌트로 변환할 때 (`@svgr/webpack` 사용 등), 변환된 컴포넌트가 예상대로 동작하는지 항상 테스트하세요.
    
15. **로더 순서**: Webpack의 로더는 오른쪽에서 왼쪽 순서로 적용됩니다. 로더의 순서가 올바른지 확인하고, 필요한 설정이 제대로 적용되고 있는지 검증하세요.

---

## Tailwind CSS 설정시 주의해야 하는 것들

1. **설정 오버라이드**: Tailwind의 기본 설정을 확장하려면 `extend` 옵션을 사용해야 합니다. 직접적으로 설정을 변경하면 기본 설정이 완전히 덮어씌워질 수 있으므로 주의가 필요합니다.
   
3. **파일 크기**: 너무 많은 커스텀 유틸리티나 변형을 추가하면 생성된 CSS 파일의 크기가 커질 수 있습니다. 반드시 필요한 변형만 추가하고, 사용하지 않는 변형은 `variants` 설정에서 제거하세요.
   
5. **Purge 설정**: `purge` 옵션을 사용하여 미사용 CSS를 제거할 수 있습니다. 이 설정을 올바르게 하지 않으면 프로덕션 빌드에서 필요한 스타일이 누락될 수 있습니다. 항상 경로와 패턴을 올바르게 지정하고, 프로덕션 빌드 후에 스타일이 올바르게 적용되는지 확인하세요.
   
7. **플러그인 사용**: Tailwind 플러그인을 사용할 때, 호환성 및 중복 스타일 문제를 주의하며 항상 최신 버전의 플러그인을 사용하도록 하세요.
   
9. **컬러 팔레트**: 사용자 정의 색상을 추가할 때, 색상의 명도 및 채도를 고려하여 일관성 있는 팔레트를 유지하는 것이 중요합니다.
    
11. **커스텀 유틸리티**: 주석 추가 - "새로운 유틸리티를 추가할 때, 목적에 맞는 네이밍 컨벤션을 사용하고, 기존의 일관된 스타일과 조화를 이루도록 유지하는 것이 중요합니다."
    
13. **JIT 모드**: 주석 추가 - "JIT 모드를 사용할 때, 스타일을 개발 중에 실시간으로 생성하여 빠른 개발을 지원합니다. 그러나 몇몇 환경에서는 문제가 발생할 수 있으므로 사용하기 전에 테스트를 통해 가능한 문제점을 사전에 파악하는 것이 좋습니다."
    
15. **브라우저 호환성**: 주석 추가 - "Tailwind CSS는 대부분의 현대 브라우저에서 잘 동작하지만, 구버전 브라우저에서는 추가 설정이 필요할 수 있습니다. 특히 IE11과 같은 브라우저에서는 호환성 문제가 발생할 수 있으므로 주의가 필요합니다."
    
17. **속성 순서**: 주석 추가 - "CSS 속성의 순서는 렌더링에 영향을 미칠 수 있습니다. Tailwind 클래스의 순서를 일관되게 유지하여 스타일이 예기치 않게 변경되는 상황을 방지하세요."
    
19. **충돌**: 주석 추가 - "다른 CSS 프레임워크나 라이브러리와 함께 사용할 경우, 클래스 이름이나 스타일 충돌이 발생할 수 있습니다. 충돌을 최소화하고 해결하기 위한 방법을 명시하는 것이 중요합니다."

